# Plugins Deep Dive 

## 1. Executive summary

- Plugins are plain JVM classes packaged as JARs and dropped into a configurable plugin folder; the backend scans that directory on startup and instantiates every non-abstract class that directly extends `AnalyzerAbstractPlugins`.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L66-L172】【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L415-L497】
- Each plugin declares one or more `FindingType`s (id, severity, message template, etc.) and a target scope via `@PluginTarget(ObjectType…)`; the interpreter uses these to decide which model elements to feed into the plugin and later stamps severity back onto produced findings.【F:src/main/java/ssee/twxanalyzerbackend/api/TwxInterpreterAbstract.java†L26-L44】【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L545-L636】【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L878-L894】
- Plugin configuration items are collected from every plugin’s `getPluginDefaultConfigItems()`, merged with the database, and pushed back into each plugin during reload so runtime configuration stays in sync with JAR defaults.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L308-L358】【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L699-L744】
- Analysis runs per object type (PROCESS, BPD, ENVIRONMENTVARIABLESET, etc.): for each target type the interpreter iterates every element representation, calls `initialize → performAnalysisGetFindings → teardown`, and then buckets findings by `findingTypeId`.【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L773-L831】
- Plugin enablement is per project/toolkit snapshot via `AppToPlugin` records; findings for unchecked finding types are stripped after execution so a multi-finding plugin can be partially enabled.【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L554-L662】【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L833-L855】
- Finding types are stored in MongoDB; loader compares JAR-defined types with DB contents to add new ones, update metadata, and flag removed ones for cleanup without deleting historical findings.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L113-L211】【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L174-L180】
- The plugin build aggregates (`subtree-plugin-*`) output versioned JARs named after their module into `plugins/`; the folder can be overridden at runtime with `-Dsystem.overwrite.plugins.folder`.【F:subtree-plugin-coba-ekp/_buildPlugins-ekp/pom.xml†L10-L199】【F:subtree-plugin-coba-ekp/ekp-plugin_entry-and-exit-logging/pom.xml†L15-L36】【F:src/main/java/ssee/twxanalyzerbackend/core/services/ConfigurationService.java†L612-L624】
- Base analyzers (built-in “internal” plugins) are registered programmatically and run alongside external JAR plugins to produce shared data such as ontology matches, notes, and toolkit conflict scaffolding.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L604-L611】【F:subtree-plugin-global/global-plugin_internals-container/src/main/java/de/ssee/analyzer/plugin/GlobalPluginInternalsToolkitConflict.java†L17-L48】

## 2. Plugin lifecycle: from jar to findings

1. **Jar placement & discovery**  
   - Plugin JARs are produced by module POMs that set `finalName=${project.artifactId}-${project.version}` and write into `${plugins.targetfolder}` (resolved to `../../plugins` in the aggregator).【F:subtree-plugin-coba-ekp/ekp-plugin_entry-and-exit-logging/pom.xml†L15-L36】【F:subtree-plugin-coba-ekp/_buildPlugins-ekp/pom.xml†L10-L199】
   - At startup `PluginLoaderService.init()` initializes base plugins, scans the plugin folder (which may be overridden by `system.overwrite.plugins.folder`), and loads every `.jar` file found there.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L66-L71】【F:src/main/java/ssee/twxanalyzerbackend/core/services/ConfigurationService.java†L612-L624】

2. **Class discovery / instantiation**  
   - Each JAR is opened and every `.class` entry is attempted via a `URLClassLoader` that uses the interpreter’s classloader as parent. Only concrete, non-interface classes assignable to `AnalyzerAbstractPlugins` are instantiated; helper classes in the same JAR are skipped.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L415-L497】

3. **FindingType validation & persistence**  
   - The loader verifies that each plugin exposes at least one `FindingType` with id, title, severity, etc.; invalid plugins are removed from the set. It then merges JAR-defined finding types with MongoDB (add new, update changed fields, mark removed for cleanup).【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L113-L211】【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L500-L602】

4. **Configuration bootstrap**  
   - Default config items are pulled from every plugin and base analyzer, aggregated, and saved to the plugin configuration document if missing; otherwise the DB values are compared against defaults and updated/extended with any new items.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L308-L358】

5. **Execution & findings aggregation**  
   - During analysis the interpreter filters plugins by `@PluginTarget` and project-level enablement, executes them per matching element, and aggregates findings by `findingTypeId`, adding severity metadata from the registered `FindingType`.【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L545-L636】【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L773-L894】

## 3. Running the analyzer on “each file”

“File” refers to the parsed snapshot elements stored as `BaseTypeFileRepresentationInterface` instances, keyed by `ObjectType` (PROCESS, BPD, UNDERCOVERAGENT, ENVIRONMENTVARIABLESET, etc.) inside `processAppFiles`.

**Execution sequence**
1. Load `processAppFiles` for the process app or toolkit (map of `ObjectType -> List<BaseTypeFileRepresentationInterface>`).
2. Determine plugin targets from `@PluginTarget` and check project enablement via `AppToPlugin` finding-type flags.【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L545-L662】
3. For each supported `ObjectType`, iterate every element:
   1. `plugin.initialize(element, processApp)`
   2. `plugin.performAnalysisGetFindings(snapshotId)`
   3. `plugin.teardown()`
   4. Drop findings whose `findingTypeId` is unchecked for this project.
   5. Stamp toolkit/app metadata and bucket findings by `findingTypeId` for later persistence.【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L773-L875】
4. After all plugins finish, resolve severities from the registered finding types and attach them to each finding.【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L878-L894】

## 4. Plugin targeting with annotations

- Plugins declare their intended model scopes using `@PluginTarget(type = {ObjectType.PROCESS, ObjectType.BPD, ...})`. The interpreter reads the annotation via reflection and only invokes the plugin for matching `ObjectType` entries.【F:src/main/java/ssee/twxanalyzerbackend/api/TwxInterpreterAbstract.java†L26-L44】【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L592-L613】
- Inside `performAnalysisGetFindings` plugins typically inspect the runtime type of `singleFileRepresentationElement` (set during `initialize`) using `instanceof` to branch per element representation (e.g., processes vs. environment variable sets).【F:subtree-plugin-global/global-plugin_log-info-debug-is-used/src/main/java/de/ssee/analyzer/plugin/GlobalPluginLogInfoDebugIsUsed.java†L55-L105】【F:subtree-plugin-global/global-plugin_environment-variables/src/main/java/de/ssee/analyzer/plugin/GlobalPluginEnvironmentVariables.java†L53-L132】

**Pseudocode sketch**
```java
List<ObjectType> targets = readAnnotationOn(plugin); // from TwxInterpreterAbstract
for (ObjectType type : targets) {
    for (BaseTypeFileRepresentationInterface element : processAppFiles.get(type)) {
        plugin.initialize(element, processApp);
        List<Finding> findings = plugin.performAnalysisGetFindings(snapshotId);
        plugin.teardown();
        // post-filter & bucket findings...
    }
}
```

## 5. FindingTypes and how they surface in the UI

- Plugins build `FindingType` definitions (id, severity, titles, messages, solutions, header images) using the fluent `FindingTypeBuilder` provided by the SDK, and register them during construction.【F:subtree-plugin-global/global-plugin_log-info-debug-is-used/src/main/java/de/ssee/analyzer/plugin/GlobalPluginLogInfoDebugIsUsed.java†L36-L53】【F:subtree-plugin-global/global-plugin_environment-variables/src/main/java/de/ssee/analyzer/plugin/GlobalPluginEnvironmentVariables.java†L36-L50】
- The loader persists these definitions to MongoDB, updates existing rows when fields change, and later uses them to populate `Finding` severity in-memory, ensuring UI consistency even if plugins omit severity at runtime.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L113-L211】【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L878-L894】
- Plugins can expose multiple finding types; enablement is tracked per finding type, so a single plugin may yield partially suppressed output depending on project configuration.【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L647-L662】【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L833-L855】

## 6. Plugin configuration

- **Defaults**: Each plugin may return a list of `ConfigItem` objects via `getPluginDefaultConfigItems()`, specifying UI control type, label, placeholder, grouping label (`groupBy`), default value(s), and the owning `findingTypeId`. Example: a regex text box for legacy logging detection is grouped under “Usage Of Old Log-Function.”【F:subtree-plugin-global/global-plugin_log-info-debug-is-used/src/main/java/de/ssee/analyzer/plugin/GlobalPluginLogInfoDebugIsUsed.java†L140-L168】
- **Aggregation & persistence**: On startup the loader collects all config items, compares them with the DB document, updates changed fields, and appends new items; if no document exists it saves the aggregated defaults as the baseline configuration.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L308-L358】
- **Runtime injection**: During `reload()` the loader filters the stored config items by the plugin’s finding types and calls `plugin.overwriteDefaultConfig(...)` so plugins can read user-specified values during analysis; base plugins receive their configs by class name matching.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L699-L744】
- **Folder configuration**: The plugin directory can be set through the system configuration (`SYS_FOLDER_PLUGINS`) and overridden with the JVM property `system.overwrite.plugins.folder`, enabling externalized deployments.【F:src/main/java/ssee/twxanalyzerbackend/core/services/ConfigurationService.java†L612-L624】

## 7. “Internalization” for flexibility (design recommendations)

- **Separate rule logic from framework glue**: Keep `AnalyzerAbstractPlugins` subclasses thin—use helpers for parsing/model traversal so the plugin class mainly wires config, initializes state, and maps matches to `Finding` objects.
- **Composable matchers/utilities**: Extract reusable finders (e.g., regex scanners, path navigators) into shared libraries so multiple plugins can compose them without duplicating `instanceof` cascades.
- **Data-driven rules**: Where possible, encode check criteria as config (regex lists, severity overrides, whitelist/blacklist IDs) and let a generic matcher plugin interpret them, reducing new code for each variant.
- **Version/compatibility**: Avoid breaking `FindingTypeId` changes; if necessary, ship migration logic in `PluginLoaderCleanupService` and preserve historical IDs for already analyzed snapshots (mirroring the current “no deletion” policy).【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L174-L211】
- **Testing strategy**: Unit-test plugin logic against stub `BaseTypeFileRepresentationInterface` implementations; integration-test discovery and config merging by dropping a test JAR into the plugin folder and asserting `PluginLoaderService` writes expected finding types/config items (see existing integration test scaffolding).
- **Base vs. external plugins**: Keep cross-cutting analyses (toolkit conflicts, ontology matches, shared lists) in base analyzers so external plugins can consume their outputs instead of re-querying models.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L604-L611】【F:subtree-plugin-global/global-plugin_internals-container/src/main/java/de/ssee/analyzer/plugin/GlobalPluginInternalsToolkitConflict.java†L17-L48】

## 8. Code organization and repository layout

- **Distribution layout**: Aggregator POMs under `subtree-plugin-*` build all plugins and emit JARs into the top-level `plugins/` directory; jar names follow the module id plus version (e.g., `global-plugin_log-info-debug-is-used-3.0.5.jar`).【F:subtree-plugin-coba-ekp/_buildPlugins-ekp/pom.xml†L10-L199】【F:subtree-plugin-coba-ekp/ekp-plugin_entry-and-exit-logging/pom.xml†L15-L36】
- **Plugin sources**: Individual plugin modules live under `subtree-plugin-*/*-plugin_*` with standard Maven structure (`src/main/java`, resources, tests). Finding IDs and class names typically mirror the artifactId to ease tracing in logs and UI.
- **Internal plugins/base analyzers**: Core-only analyzers reside in `src/main/java/ssee/twxanalyzerbackend/core/baseplugins` and are registered programmatically on startup; they ship inside the server JAR rather than the external `plugins/` folder.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L604-L611】
- **Runtime registry**: Loaded plugin instances and base analyzers are kept in memory by `PluginLoaderService`; finding-type metadata is persisted in MongoDB for UI rendering and severity lookup even when the plugin JAR is absent.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L113-L211】【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L878-L894】

## 9. Appendix

### Example plugin jar inventory (sample)

| Artifact (module) | Target types | Primary finding title | Notes |
| --- | --- | --- | --- |
| `global-plugin_log-info-debug-is-used` | PROCESS | “Usage Of Old Log-Function” | Regex-configurable scan for legacy `log.info/debug/error` usage.【F:subtree-plugin-global/global-plugin_log-info-debug-is-used/src/main/java/de/ssee/analyzer/plugin/GlobalPluginLogInfoDebugIsUsed.java†L26-L168】 |
| `global-plugin_environment-variables` | ENVIRONMENTVARIABLESET | “Environment Variable Setting” | Flags missing default/stage-specific env var values across toolkits and apps.【F:subtree-plugin-global/global-plugin_environment-variables/src/main/java/de/ssee/analyzer/plugin/GlobalPluginEnvironmentVariables.java†L24-L132】 |
| `global-plugin_internals-container` | UNKNOWN (internal) | “Toolkit Conflict” et al. | Registers finding types whose payloads are produced by internal/base analyzers (e.g., toolkit version conflicts).【F:subtree-plugin-global/global-plugin_internals-container/src/main/java/de/ssee/analyzer/plugin/GlobalPluginInternalsToolkitConflict.java†L17-L48】 |
| `ekp-plugin_entry-and-exit-logging` | PROCESS | EKP-specific logging rule | Built via EKP aggregator; jar lands in `plugins/` with versioned name.【F:subtree-plugin-coba-ekp/ekp-plugin_entry-and-exit-logging/pom.xml†L15-L36】 |
| `global-plugin_snapshot-naming` | PACKAGEDATA | Snapshot naming convention check | Packaged alongside EKP build as shared/global plugin.【F:subtree-plugin-coba-ekp/_buildPlugins-ekp/pom.xml†L133-L198】 |

### Glossary

- **Snapshot**: Parsed export of a process app or toolkit; provides `snapshotId` and element maps consumed by plugins.
- **ObjectType** / **ElementType**: Enumeration of model element categories (PROCESS, BPD, UNDERCOVERAGENT, ENVIRONMENTVARIABLESET, etc.) used to route elements to plugins.【F:src/main/java/ssee/twxanalyzerbackend/api/TwxInterpreterAbstract.java†L26-L44】
- **Finding**: Concrete issue instance produced by a plugin, tied to a `findingTypeId`, enriched with template values and later severity metadata.【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L773-L894】
- **FindingType**: Metadata definition (id, severity, titles, descriptions) registered by plugins and stored in MongoDB for UI rendering and severity lookup.【F:src/main/java/ssee/twxanalyzerbackend/core/services/PluginLoaderService.java†L113-L211】
- **Severity**: Importance level (Error/Warning/BadStyle, etc.) attached to finding types and applied post-analysis before persistence.【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L878-L894】
- **Toolkit/ProcessApp**: IBM BPM/BAW package types; both are analyzed, with toolkit conflicts surfaced through internal/base plugins plus corresponding finding types.【F:src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java†L195-L216】【F:subtree-plugin-global/global-plugin_internals-container/src/main/java/de/ssee/analyzer/plugin/GlobalPluginInternalsToolkitConflict.java†L17-L48】
