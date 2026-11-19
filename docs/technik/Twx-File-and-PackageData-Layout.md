## TWX package layout (example: `TwxAnalyzer_Beispiel_App - 07042020_34_TWXBAPP.twx`)

- The archive is a ZIP with domain-specific extensions and contains `META-INF/package.xml`, an `objects/` folder, optional `files/` content, and dependency toolkits under `toolkits/`.

### `package.xml`

- Root `<p:package>` element lists build metadata and several logical sections.
- `<target>` identifies the project, branch, and snapshot being exported.
  - Example: `<project ... name="TwxAnalyzer Beispiel App" shortName="TWXBAPP"/>`
- `<dependencies>` enumerates required toolkits; each entry references a project, branch, and snapshot and corresponds to a ZIP in `toolkits/` named after the snapshot id (e.g., `toolkits/2064.9c779d7b-9b3e-3ad4-8871-548a47bb3d57.zip`).
- `<objects>` lists every artifact in the snapshot. Each object has an `id`, `versionId`, `name`, and `type`.
  - The `id` matches the filename under `objects/` with a `.xml` suffix (e.g., `id="1.36559412-8d2b-4c77-974c-00cc42f0b4f2"` → `objects/1.36559412-8d2b-4c77-974c-00cc42f0b4f2.xml`).
  - The `type` indicates the artifact category (`process`, `bpd`, `underCoverAgent`, `twClass`, `trackingGroup`, `participant`, `resourceBundleGroup`, `externalActivity`, `managedAsset`, etc.).
- `<files>` connects binary payloads to object ids.
  - Each `<file>` entry uses `id` to point back to an `objects` entry and `path` to reference the stored blob under `files/<id>/<path>`.
  - Example mapping: `<object id="61.2d67fc59-d560-49c4-8557-296c846ad0ad" ... name="e77f2a7e-10b4-45ee-90eb-e5b1546cc743.zip" type="managedAsset"/>` pairs with `<file path="0213ab72-ab5c-456e-991e-2d5d48ff903f" id="61.2d67fc59-d560-49c4-8557-296c846ad0ad"/>` and the actual payload at `files/61.2d67fc59-d560-49c4-8557-296c846ad0ad/0213ab72-ab5c-456e-991e-2d5d48ff903f`.

### Folder structure (high level)

- `META-INF/package.xml` — manifest describing targets, dependencies, objects, and file references.
- `objects/` — one XML file per object, named `<object-id>.xml`; contents define the artifact but are not needed to understand the structure.
- `files/` — optional binary assets organized by object id; file names are GUID-like paths listed in `<files>` and derive their human-friendly label from the corresponding object `name`.
- `toolkits/` — dependency archives; filenames match snapshot ids listed in `<dependencies>`.

## How the analyzer maps a TWX snapshot into runtime classes

- The analyzer loads the TWX ZIP from MongoDB, parses it into `XmlProcessApp`, and then maps it to a `ProcessAppImpl` instance via `MyProcessAppMapper` (this mapping step also triggers post-processing to enrich IDs and references).【F:src/main/java/ssee/twxanalyzerbackend/api/Analyzer.java†L190-L275】【F:src/main/java/ssee/twxanalyzerbackend/mapper/processApp/MyProcessAppMapper.java†L39-L274】
- `ProcessAppImpl` is the in-memory representation of a process app or toolkit. It stores:
  - `toolkits`: a map of toolkit snapshots (`snapshotId` → `ProcessAppInterface`) so dependencies can be resolved recursively.【F:src/main/java/ssee/twxanalyzerbackend/ext/ProcessAppImpl.java†L53-L276】
  - `processAppFiles`: a map of `ObjectType` → list of `BaseTypeFileRepresentationInterface` implementations, holding every parsed object from `objects/` + `package.xml` metadata.【F:src/main/java/ssee/twxanalyzerbackend/ext/ProcessAppImpl.java†L58-L240】

## Object types and their file representation classes

`ProcessAppImpl` stores everything in `processAppFiles` and the mapper decides which concrete class to use for each XML object type. The current mapping in `MyProcessAppMapper` is:

| XML object type | Stored `ObjectType` | File representation class (in `ssee.twxanalyzerbackend.ext.objects.*`) |
| --- | --- | --- |
| `PROCESS` | `ObjectType.PROCESS` | `ProcessTypeFileRepresentationImpl` |
| `WEBSERVICE` | `ObjectType.WEBSERVICE` | `WebServiceTypeFileRepresentationImpl` |
| `BPD` | `ObjectType.BPD` | `BpdTypeFileRepresentationImpl` |
| `PARTICIPANT` | `ObjectType.PARTICIPANT` | `ParticipantTypeFileRepresentationImpl` |
| `TWCLASS` | `ObjectType.TWCLASS` | `TwClassTypeFileRepresentationImpl` |
| `COACHVIEW` | `ObjectType.COACHVIEW` | `CoachViewTypeFileRepresentationImpl` |
| `UNDERCOVERAGENT` | `ObjectType.UNDERCOVERAGENT` | `UcaTypeFileRepresentationImpl` |
| `EPV` | `ObjectType.EPV` | `EpvTypeFileRepresentationImpl` |
| `PACKAGEDATA` | `ObjectType.PACKAGEDATA` | `PackageDataTypeFileRepresentationImpl` |
| `RESOURCEBUNDLEGROUP` | `ObjectType.RESOURCEBUNDLEGROUP` | `ResourceBundleGroupTypeFileRepresentationImpl` |
| `ENVIRONMENTVARIABLESET` | `ObjectType.ENVIRONMENTVARIABLESET` | `EnvVariableSetTypeFileRepresentationImpl` |
| `ARTIFACT` | `ObjectType.ARTIFACT` | `ArtifactTypeFileRepresentationImpl` |
| `EXTERNALACTIVITY` | `ObjectType.EXTERNALACTIVITY` | `ExternalActivityTypeFileRepresentationImpl` |
| `MANAGEDASSET` | `ObjectType.MANAGEDASSET` | `ManagedAssetTypeFileRepresentationImpl` |
| `METADATA` | `ObjectType.METADATA` | `MetaDataTypeFileRepresentationImpl` |
| `CONTRIBUTION` | `ObjectType.CONTRIBUTION` | `ContributionTypeFileRepresentationImpl` |
| `PROJECTDEFAULTS` | `ObjectType.PROJECTDEFAULTS` | `ProjectDefaultsTypeFileRepresentationImpl` |
| `USERATTRIBUTEDEFINITION` | `ObjectType.USERATTRIBUTEDEFINITION` | `UserAttributeTypeFileRepresentationImpl` |

This mapping shows which object IDs in `objects/` end up in which strongly-typed Java objects and which `ObjectType` bucket they live in inside `ProcessAppImpl`.【F:src/main/java/ssee/twxanalyzerbackend/mapper/processApp/MyProcessAppMapper.java†L67-L257】

### Base type metadata

Every `*TypeFileRepresentationImpl` inherits from `BaseTypeFileRepresentationImpl`, which stores the owning snapshot ID and whether the object comes from a toolkit. During `postActions`, `ProcessAppImpl` stamps each object with the current snapshot and toolkit metadata (using `updateAppOrTkSnapshotIdAndToolkitInfo`).【F:src/main/java/ssee/twxanalyzerbackend/ext/BaseTypeFileRepresentationImpl.java†L3-L21】【F:src/main/java/ssee/twxanalyzerbackend/ext/ProcessAppImpl.java†L540-L560】

## Dependencies and toolkits

- Dependency data from `<dependencies>` is stored in `PackageDataTypeFileRepresentationImpl` via `dependentToolkits`, while `<objects>` is tracked as `packageDataObjectsById`, and `<files>` is captured as `lstFiles`. This class represents the parsed `package.xml` data inside the application model.【F:src/main/java/ssee/twxanalyzerbackend/ext/objects/packagedata/PackageDataTypeFileRepresentationImpl.java†L29-L68】
- `ProcessAppImpl.getToolkitByDependencyId` resolves a toolkit by its dependency ID, normalizing IDs with the `2069.` prefix before looking them up in the package data and then matching that dependency to a toolkit snapshot in the `toolkits` map.【F:src/main/java/ssee/twxanalyzerbackend/ext/ProcessAppImpl.java†L256-L275】
- `getToolkitBySnapshotId` and `getAllPackageDataTypeFiles` provide the other key lookup paths for navigating between app snapshots and toolkit snapshots, which is how the analyzer follows `toolkits/` ZIPs back to their package metadata.【F:src/main/java/ssee/twxanalyzerbackend/ext/ProcessAppImpl.java†L240-L402】

## File sections and special metadata objects

- The `PACKAGEDATA` object (from `package.xml`) is treated as a first-class object type in `processAppFiles`, and `ProcessAppImpl.getPackageDataType()` exposes it for the rest of the analyzer to use when resolving snapshot metadata and dependency objects.【F:src/main/java/ssee/twxanalyzerbackend/ext/ProcessAppImpl.java†L360-L381】
- The `METADATA` object (also parsed into `processAppFiles`) contains tags. `ProcessAppImpl.getTags()` reads this metadata to decide whether a given element should be excluded from analysis (based on tag groups defined in metadata).【F:src/main/java/ssee/twxanalyzerbackend/ext/ProcessAppImpl.java†L500-L623】
- Resource bundle files (`RESOURCEBUNDLEGROUP`) are also parsed as object types, and `ProcessAppImpl` builds lookup maps for:
  - Resource bundle names (`resourceBundleGroupId` → bundle name).
  - Resource bundle keys as `tw.resource.<bundle>.<key>` strings for quick lookup during analysis (including toolkits).【F:src/main/java/ssee/twxanalyzerbackend/ext/ProcessAppImpl.java†L284-L739】
