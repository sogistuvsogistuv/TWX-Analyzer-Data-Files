# Index

# Welcome to the twx-Analyzer documentation

## Release highlights

!!! important "Latest Release 2.3.6"
Permanently ignore findings from the result list [short description](./components/analyze.md#ignore-findings)

!!! important "Release 2.3.1"
    Git like comparison between two Snapshots utilizing the snapshots XML. [Detailed description](./components/explorer.md#details-snapshot-diffs)   
    Orphan Toolkit overview [short description](./components/explorer.md#detail-orphan-admin)

!!! success "Release 2.3.0"
    New CICD endpoint for acronym instead of id usage [spec for ci integration (yaml-File)](./installation/openapi_ci_v2.3.0.yaml)

!!! success "Release 2.2.0"
    twx-Analyzer installation via Helm-Chart

!!! success "Release 2.1.0"
    With this release we introduced the first version of a [full text search](components/developer/#details-fulltext-search).

## About the project
Code quality, adherence to guidelines and good practices are the major challenges of any BPM project. As the headcount of the project and lines of code grow, so do these challenges. Overall, application quality declines: Code gets harder to maintain, application performance suffers, defects become more difficult to find and deployments become more error-prone.

Existing code analysis tools often lack the flexibility to be tailored to individual coding guidelines. The twx-Analyzer contains a flexible plugin architecture and is developed considering, best practice rules alongside many projects.

## General description
The twx-Analyzer is a code governance and code analysis tool to help throughout the entire development cycle in IBM BPM projects. It thrives to solve some known shortcomings of the IBM Process Designer. But most important the analyzer focuses on helping to maintain development rules and good practices. It is also designed to help during refactoring tasks, which is one of the most important tasks in maintaining a clean code.

These tasks are accomplished by a plugin based system. Plugins can be created and customized for project or customer specific needs. All available plugins are listed [here](plugins/overview.md).

## Capabilities
Plugins create the analysis result and deliver so called 'findings', which indicate that some code or configuration disobeys a certain rule and therefor a developer should attend to and change it.But the twx-Analyzer also delivers tools and functionality for other tasks and questions, that come in handy when developing with IBM BPM. Details are at the components section.

Whenever you find a search function on top of a table, with the key combination "ctrl + shift + f" you put the cursor to the search and filter the entire table. Some tables also have a column specific filter function. Here you can find filter functions like "not equal", "not contains".

## License export 
Under Settings -> General you can download the license pdf file if your copy of the twx-Analyzer is not a demo version.

---

# Overview

# Overview installation guide

<!-- ## Download

You can download the twx-Analyzer with your account at [URL](www.myDownloadServer.com). TODO -->

## Basic information

The twx-Analyzer is based on Spring boot, therefor it does already contain its own runtime environment. To run the twx-Analyzer, the operating system must provide Java and a MongoDB. The demo version is an exception, it does not need a MongoDB. For detailed information about system requirements, please see requirements section below.

But there is also the possibility to use it on openshift. A proper Helm-Chart is available for that.

## Standalone Server 

### [Install server](installing.md)

An installation of the twx-Analyzer is not necessary. Simply unzip the delivered twx-Analyzer-*.zip file into the destination folder.
In the below sections you will find information on how to install prerequisites like MongoDB and Java.

### [Starting server](start-server.md)

After a successful setup of all prerequisites, the twx-Analyzer is ready to start. For a final check you can use these commands:

- java -version
- mongo -version

In case your are about to start a demo version, you can check your java version with:

- java -d64 -version

Please keep in mind, that the demo version requires a 64-bit java installation.

!!! tip
    Every time the demo version is stopped, all analyzed data will be removed. 

### [Configuration](configuration.md)

This section contains description about additional startup parameters, like configuring different ports or adding certificates, ...

### Requirements

    1. Supported MongoDB Versions 
    1.1 Without using full text search
        - V7.0
        - V6.0
        - V5.0
        - V4.0
        - V3.X
    1.2 When using full text search
        - V7.0
        - V6.0
        - V5.0

    2. Java Runtime version 17
        - Demo requires 64-bit Java
        
    3. Firefox version 129 or higher
    4. Internet Explorer is NOT supported
    5. Microsoft Edge is NOT fully supported

## Helm Installation

We recommend you to use OpenShift when deploying TWX-Analyzer to production. You can install the Helm charts by running:

`helm install <YOUR HELM RELEASE NAME> ./twx-analyzer --set route.host=<twx-analyzer DNS name> --set pvc.storageClass=<storage class name>`

### Configure Charts

Before the charts can be installed, the corresponding configuration files (**twx-analyzer/configs/**)
must be adapted according to the infrastructure requirements:

- *ldap/security.properties* - please enter the appropriate parameters for the connection to the
LDAP Server (Microsoft Active Directory)
- *keystore/twxAnalyzer_keystore.p12* - Please add the certificates required for your
infrastructure to the wxAnalyzer_keystore.p12 keystore (e.g. for a connection to the LDAP
server)
- *certs/tls.crt,tls.key,ca.crt* - Please add the certificate/key pair and related CA certificate
required for the Route resource with a custom certificate and re-encrypt TLS termination.

We recommend to setup the connection to the BAW server in order to use all features. Therefor set the secrets for `baw_technical_user` and `baw_technical_user_pw` in the secret.yaml of the backend (analyzer) `twx-analyzer/templates/analyzer/secret.yaml`.

#### Timeouts

The default timeout settings for the nginx server are:
```nginx configuration
    # Avoid 504 HTTP Timeout Errors
    proxy_connect_timeout       605;
    proxy_send_timeout          605;
    proxy_read_timeout          605;
    send_timeout                605;
    keepalive_timeout           605;
```

These are set in the default.conf. One may change theses values according to their needs. Especially in project with huge exports this might be necessary to further increase. 

### Uninstalling Charts

You can remove these charts by running:
`helm uninstall <YOUR HELM RELEASE NAME>`

### Requirements

There are none specifically requirements. But keep in mind that Database needs enough space. 

## Integration in build automation system

To analyze a snapshot without any UI interaction, the twx-Analyzer offers a rest API for uploading, downloading and analyzing
snapshots.

Please check below OpenApi specification to configure automatic analysis.

If you start up the twx-Analyzer in a container to analyze the twx files, keep in mind that there is yet no
api endpoint to configure the application. Either you use an existing instance of the analyzer, or you choose to use 
the default configuration as well as all plugins from the plugin folder. 

With release 2.3.0 there a new endpoints, that can handle acronyms instead of the ids.
This is the **current** OpenApi specification yaml-file: [spec for ci integration (yaml-File)](./openapi_ci_v2.3.0.yaml).


Change list:

    * Capability to analyze and download snapshots by using the acronyms instead of ids
    * New api documentation with OpenApi 3.0
    * Added error response objects
    * Better distinguish between downloading a specific snapshot, 
    automatic download of the latest snapshot and the possibility to include the analysis

With release 2.1.0 there a new endpoints, and the path variables slightly changed.
This is the previous OpenApi specification yaml-file: [spec for ci integration (yaml-File)](./openapi_ci_v1.2.yaml).

The endpoints of the [previous release 2.0.6](./openapi_ci_v1.0.yaml) are no longer supported.

---

# Overview

# Overview

<!-- TODO auch bei capabilities auf der welcome page -->
Plugins create the analysis result and deliver so called ’findings’, which indicate that some code
or configuration disobeys a certain rule and therefor a developer should attend to and change
it. But the twx-Analyzer also delivers tools and functionality for other tasks and questions,
that come in handy when developing with IBM BPM. The following sections explain the basic
capabilities.



## [Start](startsection.md)

This section is the tool to manage, upload and analyze snapshots. It is the control center for
snapshots.

* Snapshots: Lists all analysed snapshots and shows the current activated snapshot.

* Upload: You can upload and start analysis of BAW and ISF files.

## [Analyze](analyze.md)

List the findings of the currently selected snapshot in different categories.

## [Develop](developer.md)

There you can find tools which are helpful for the day to day development tasks.

## [Explore](explorer.md)

It contains tools for a better overview into a snapshot and contains search functionalities.

## [Settings](settings.md)

The system and plugin configuration is editable in this section. It also contains the overviews off available plugins.

---

# Overview

# Overview standard plugins

Plugins are created to find errors, warnings, bad style implementations. 

Basically there are two types of plugins. Those which are tightly bundled with the product and those which were customized to your project needs. Below topics cover all currently available plugins, that are included in the standard installation. A list of plugins, which are available in your current twx-Analyzer setup, is accessible at the settings tab in subsection "plugins" of the twx-Analyzer.

One of the main aspects of the twx-Analyzer is that projects or institutions can develop their own plugins. Please contant us if you plan to do so. <!--[develop their own plugins](../plugindevelopment/overview.md).-->

## [Error Plugins](details/error-plugins.md)

Most error findings would result in a runtime error.

## [Warning Plugins](details/warning-plugins.md)

Warnings may be performance related or could be cases, that do not follow your [good practices](../bestpractices/all.md).

## [Bad Style Plugins](details/badstyle-plugins.md)

This simply contains findings, which do not follow certain style guides or common agreements.

## Other Plugins

These types of plugins are informational list, which do not necessarily point towards a problem. They could be used to extract certain recurring information. So far none of these plugins are available with the standard delivery.


## More information
### Ignore findings
In case of 'false positives' or for some reason the results of a specific finding type should be ignored, the analyzer uses tagging to accomplish this task. Simply add a tag group named "TwxAnalyzerIgnores" to your project and inside that tag group use the title of the plugin as the name for the tag. Set this tag to an object and the results will not be generated for this.

I case an element should always be ignored, provide the tag 'all' inside the tag group.



---

# Overview

# Overview of Plugin implementation

To create a new plugin register an account with download.twx-analyzer.de. There you can download a sample project to create a new plugin project.

The new project will contain a lib-folder in which the latest "Development Kit" (analyzer-SDK.jar) resides. This lib is only needed during development and should not be packaged with the new plugin jar.

The next step is to decide which ElementTypes your plugin can handle and anotate the Class with the correct elements:

```Java
@PluginTarget(type = {ObjectType.PROCESS, ObjectType.BPD})
```

Each plugin can create several so called [findingTypes](../../components/analyze.md#Findingtypes). At first these findingsTypes need to be defined. Use the FindingTypeBuilder in class FindingType. In the end, add the new FindingType to the list of all findingTypes via the method 'getLstFindingTypen()'.


This is an example:
```Java
// SETUP YOUR NEW FINDING TYPE
FindingType findingTyp =
    new FindingType.FindingTypeBuilder()
        .setLongDescription(
            "Description of the FindingType so that users known what issue is displayed in the below list of findings.")
        .setTitle("Readable Name Sample") // This is the readable name of the finding.
        .setFindingTypeId(SAMPLE_PLUGIN_FINDINGTYPEID)
        .setShortName(SAMPLE_PLUGIN_NAME)
        .setSeverity(Severity.ERROR)
        .setShortDescription(
            "This is the description that will be displayed if the power users mode is enabled.")
        .setMessageTemplate(
            "The item {ITEM} has an save execution context.") //
        .setHeaderImageUrl(ConstantsHeaderImageUrl.ITEMHASSAVEEXECUTIONCONTEXT)
        .setSolution(
            "Set a default value for the environment variable or to be more precise set a value for each target stage.")
        .buildFindingType();

getLstFindingTypen().add(findingTyp);
```

The analyzer runs each plugin on each element type, if the ObjectTypes (Check the enum class for the availabe different types) match. So the analyzer runs the method:
```java
List<Finding> performAnalysisGetFindings(String snapshotId);
```
on each turn.

Before the performAnalysisGetFindings method is called the initialize method is executed. Within this method the variable singleFileRepresentationElement is set. This is important since the variable is of type BaseTypeFileRepresentationInterface and may contain all sorts of classes which inherit from this base class. Each element type like bpd or process or uca inherit from this base class. So, in order to find out which element the current run is handling, check with java instanceof.

```Java
if (singleFileRepresentationElement
        instanceof ProcessTypeFileRepresentationInterface) { // Is ProcessType (Service)
```

Now you can start creating new plugins.

If the plugin is finished, upload it to the plugin folder and restart the server. The correct folder can be found in the [configuration](../../components/settings.md#general-settings) section.

## Plugin configuration

I case the plugin needs changeable settings, you can do this as well. In the sample plugin project, there is a sample.

```Java
public List<ConfigItem> getPluginDefaultConfigItems() {

    final String GROUP_NAME = "myOwnPluginGroup";
    if (configItems == null) configItems = new ArrayList<>();

    configItems.add(ConfigItem.builder()
            .name("uniqueName")
            .type(ConfigFrontendElementType.TEXT_TYPE)
            .valueText(config.getBasePlugin().getLoggernameVariableRegexp())
            .pluginClassName(this.getClass().getSimpleName())
            .findingTypeId(findingTypeId)
            .label("Regular expression for identifying the loggername content")
            .placeholder("Default: " + config.getBasePlugin().getLoggernameVariableRegexp())
            .groupBy(GROUP_NAME)
            .build());

    log.debug("Leaving with config: {}", configItems);
    return configItems;
}
```

Check the javadoc for further information

---

# Overview

# Troubleshooting

* The demo version won’t start.

```The demo version tries to download a embedded Mongo Database. You need to have an active internet connection. You can also check the log files for errors.```

* While starting the demo version an error similar to this occurred

```ERROR d.f.e.process.runtime.Starter prepare : prepare executable
java.io.IOException: Could not open inputStream for https://fastdl.mongodb.org/osx/mongodb-osx-x86_64-3.5.5.tgz
```
Solution: The application was not able to download the mongodb files. You can download those manually. Please see [Installation](../installation/installing.md)

* Where are the log files?

```The default log file directory, is named ’log’ and the folder should be next to your twx-Analyzer Jar-file.```



---

# All

## Logging Convention
Usually projects start out with the BPM internal logging mechanism. But as your project grows, you might think about advanced logging mechanisms. There is for example java.util.logging which comes with IBM BPM. You can download the sample [logging file here](./logging.js.txt). The advantage of this approach is, that your log levels are controllable through the ISC Console. And you can create a more fine grained logging, which does not clutter your log file and reduces the performance impact on your system. There are other ways to do logging, you could extract your applications logging into different files. For more information please contact a representative of the twx Solutions GmbH.

### How it works (java.util.logging)
Every service needs two additional private variables although in BAW 19 it is only one. The first variable 'serviceName' contains the service name and is set at the start script. Then it needs a loggername, for that a variable called localLoggerName needs to be initialized with a logger name that is suitable for your project. These two variables are then used by the log command. Which would look similar to this:

``` javascript hl_lines="3 4"
tw.local.serviceName = "GeneralReusableService";
tw.local.localLoggerName = "service.global";
var logMessage = "This is my message to you..."
logger.info(logMessage, tw.local.serviceName, tw.local.localLoggerName);
```

<!--
The result in the log file would be:
`TODO`
Compared to the log.info output:
`TODO`
-->

### Start- and End-Logging 
It has been proven helpful to start every service with a script that creates an info-level log entry that the service just started. The same counts for an output logging. Before every end event item, a script that creates an end logging should be introduced. This helps to a) see which service chain was called and b) a rough estimation on how long it was executed.

#### Debug 
Additionally to the info logging at the beginning and end, you should insert a debug logging, that prints all input variables at the beginning and all output variables at the end.  

Here is how it should look like: 
``` javascript hl_lines="7"
tw.local.serviceName = "GeneralReusableService";
tw.local.localLoggerName = "service.global";
if(logger.isDebug(tw.local.localLoggerName)){
    var logMessage = "InputWerte: ";
    logMessage += "analysisId = " + tw.local.analysisId;
    logMessage += ", anotherVariable = " + tw.local.anotherVariable;
    logger.debug(logMessage, tw.local.localLoggerName, tw.local.serviceName);
}
```

A tool, that generates these input and output scripts is planed for release 2.0.2

### Changes with BAW 19

With BAW 19 the variable tw.local.serviceName is no longer needed. BPM now can call its own service name

### Old log.(info|debug)

With this convention the old logging mechanism is deprecated and there will be a warning if it is used.

### Plugins with this best practice
The following plugins belong to this best practice:

* [No Entry Logging](../plugins/details/badstyle-plugins.md#no-entry-logging)

* [No Entry Logging Finer](../plugins/details/badstyle-plugins.md#no-entry-logging-finer)

* [No Entry Logging Debug](../plugins/details/badstyle-plugins.md#no-entry-logging-debug)

* [Usage Of "log.info"](../plugins/details/warning-plugins.md#usage-of-loginfo")

## UCA good practices
UCAs can be tricky, especially if they are heavily used. It is easy to loose focus on which service triggers which UCA. And in production it is even important to be able to understand what has happend. These good practices shall help improving such situations.

### UCA Missing logging

UCAs are tricky to follow and hard to analyze. Therefore it is recommended to create a logging statement whenever a UCA is either received or send. For logging if one is sent, you should create a service inside the UCA instead of using a variable and have the service create a log statement. See below picture: ![figure 3](../pictures/uca_attached_service.png "Figure 3: Uca Attached Service"). And at the receiving end you should log the message in the condition section. Don't forget to add your condition or to add a true statement at the end like in figure 4. This final 'true;' statement is crucial in order to make this work. The corresponding plugin to this best practice is called "UCA Condition Without 'return true'". This is an example:
![figure_4](../pictures/uca_attached_service_receive.png "Figure 4: Uca Logging at a receiver")

Condition example:
``` javascript
logger.finer("UCA logging example: " + tw.message.ucaVariable + " empfangen.","service.global",tw.system.process.name);
true;
```

### Plugins with this best practice
The following plugins belong to this best practice:

* [UCA Condition Without 'return true'](../plugins/details/error-plugins.md#UCA Condition Without 'return true')

## Gateways
Gateways are great if you want to make your service flow readable and not hide all your decisions in script code. But a few things are necessary to keep in mind. Gateways should always be documented and the name should be a question. Often gateways have multiple outflows and because they are poorly named developers confused the flow and insert the wrong conditions into the flow. That is why every outgoing flow of a gateway should be named as if it would answer the question.

### Plugins with this best practice
* [Switch Condition Not Implemented](../plugins/details/error-plugins.md#switch-condition-not-implemented)

## Exception Event Naming
Exceptions are not evil! At some point you have to deal with exceptions and it turned out that exceptions should be named like the service of which they originate from. The twx-Analyzer has a built-in tool, that allows you to create the exception names. It is located under section develop -> service naming. You will find detailed description about this tooling under [ServiceNaming tool](../components/developer.md)

### Plugins with this best practice
* [Exception End Event Name](../plugins/details/warning-plugins.md#exception-end-event-name)

## Naming Good Practices
Consistend naming is important for many different reasons. Those are reducint the effort for reading and understanding the code, finding elements faster and during reviews arguing about more important issues then naming syntaxes saves time and effort.

### Plugins with this best practice
* [Step Name Differs From Service Name](../plugins/details/warning-plugins.md#step-name-differs-from-service-name)
* [Element With Default Name](../plugins/details/badstyle-plugins.md#element-with-default-name)
* [Switch Output Not Named](../plugins/details/badstyle-plugins.md#switch-output-not-named)

<!-- 
## Service naming
Name services where they are used with servicename at the beginning.
-->



---

# Overview

# Overview installation guide

<!-- ## Download

You can download the twx-Analyzer with your account at [URL](www.myDownloadServer.com). TODO -->

## Basic information

The twx-Analyzer is based on Spring boot, therefor it does already contain its own runtime environment. To run the twx-Analyzer, the operating system must provide Java and a MongoDB. The demo version is an exception, it does not need a MongoDB. For detailed information about system requirements, please see requirements section below.

But there is also the possibility to use it on openshift. A proper Helm-Chart is available for that.

## Standalone Server 

### [Install server](installing.md)

An installation of the twx-Analyzer is not necessary. Simply unzip the delivered twx-Analyzer-*.zip file into the destination folder.
In the below sections you will find information on how to install prerequisites like MongoDB and Java.

### [Starting server](start-server.md)

After a successful setup of all prerequisites, the twx-Analyzer is ready to start. For a final check you can use these commands:

- java -version
- mongo -version

In case your are about to start a demo version, you can check your java version with:

- java -d64 -version

Please keep in mind, that the demo version requires a 64-bit java installation.

!!! tip
    Every time the demo version is stopped, all analyzed data will be removed. 

### [Configuration](configuration.md)

This section contains description about additional startup parameters, like configuring different ports or adding certificates, ...

### Requirements

    1. Supported MongoDB Versions 
    1.1 Without using full text search
        - V7.0
        - V6.0
        - V5.0
        - V4.0
        - V3.X
    1.2 When using full text search
        - V7.0
        - V6.0
        - V5.0

    2. Java Runtime version 17
        - Demo requires 64-bit Java
        
    3. Firefox version 129 or higher
    4. Internet Explorer is NOT supported
    5. Microsoft Edge is NOT fully supported

## Helm Installation

We recommend you to use OpenShift when deploying TWX-Analyzer to production. You can install the Helm charts by running:

`helm install <YOUR HELM RELEASE NAME> ./twx-analyzer --set route.host=<twx-analyzer DNS name> --set pvc.storageClass=<storage class name>`

### Configure Charts

Before the charts can be installed, the corresponding configuration files (**twx-analyzer/configs/**)
must be adapted according to the infrastructure requirements:

- *ldap/security.properties* - please enter the appropriate parameters for the connection to the
LDAP Server (Microsoft Active Directory)
- *keystore/twxAnalyzer_keystore.p12* - Please add the certificates required for your
infrastructure to the wxAnalyzer_keystore.p12 keystore (e.g. for a connection to the LDAP
server)
- *certs/tls.crt,tls.key,ca.crt* - Please add the certificate/key pair and related CA certificate
required for the Route resource with a custom certificate and re-encrypt TLS termination.

We recommend to setup the connection to the BAW server in order to use all features. Therefor set the secrets for `baw_technical_user` and `baw_technical_user_pw` in the secret.yaml of the backend (analyzer) `twx-analyzer/templates/analyzer/secret.yaml`.

#### Timeouts

The default timeout settings for the nginx server are:
```nginx configuration
    # Avoid 504 HTTP Timeout Errors
    proxy_connect_timeout       605;
    proxy_send_timeout          605;
    proxy_read_timeout          605;
    send_timeout                605;
    keepalive_timeout           605;
```

These are set in the default.conf. One may change theses values according to their needs. Especially in project with huge exports this might be necessary to further increase. 

### Uninstalling Charts

You can remove these charts by running:
`helm uninstall <YOUR HELM RELEASE NAME>`

### Requirements

There are none specifically requirements. But keep in mind that Database needs enough space. 

## Integration in build automation system

To analyze a snapshot without any UI interaction, the twx-Analyzer offers a rest API for uploading, downloading and analyzing
snapshots.

Please check below OpenApi specification to configure automatic analysis.

If you start up the twx-Analyzer in a container to analyze the twx files, keep in mind that there is yet no
api endpoint to configure the application. Either you use an existing instance of the analyzer, or you choose to use 
the default configuration as well as all plugins from the plugin folder. 

With release 2.3.0 there a new endpoints, that can handle acronyms instead of the ids.
This is the **current** OpenApi specification yaml-file: [spec for ci integration (yaml-File)](./openapi_ci_v2.3.0.yaml).


Change list:

    * Capability to analyze and download snapshots by using the acronyms instead of ids
    * New api documentation with OpenApi 3.0
    * Added error response objects
    * Better distinguish between downloading a specific snapshot, 
    automatic download of the latest snapshot and the possibility to include the analysis

With release 2.1.0 there a new endpoints, and the path variables slightly changed.
This is the previous OpenApi specification yaml-file: [spec for ci integration (yaml-File)](./openapi_ci_v1.2.yaml).

The endpoints of the [previous release 2.0.6](./openapi_ci_v1.0.yaml) are no longer supported.

---

# Installing

### Installation

The manual installation steps are as follows:

<!-- TODO noch viel mehr Details einarbeiten -->

- Install the MongoDB.
- Install a Java version.
- Unzip the twx-Analyzer-2.x.x.zip file into the working folder.
- Continue to [run the twx-Analyzer](start-server.md)

#### Install MongoDB

We assume that you are installing the MongoDB on the same machine as where the analyzer is running. Therefor the Database is reachable with: localhost and the default port: 27017.
In other cases please contact our support.

The twx-Analyzer creates a database called twxAnalyzernew automatically if not existend.

<!-- If the database is on another server, we recomment to use the uri-configuration:  -Dspring.data.mongodb.uri=mongodb://localhost:27017 
https://stackoverflow.com/questions/50423896/how-to-configure-ssl-mongodb-connection-in-yml-file-spring-boot
https://docs.mongodb.com/manual/reference/connection-string/
https://docs.mongodb.com/manual/tutorial/configure-ssl/
https://zappysys.com/blog/configure-mongodb-ssl-connection-certificate/
-->

#### Installation with password secured MongoDB

In case you have a password secured MongoDB Installation you need to configure a password. Please make sure the authentication is setup in the twxAnalyzernew database. Otherwise please contact our support.
In addition, if the the database is passwort protected there are two prefered options on how to start the twxAnalzyer.

Option 1: Start the analyzer with a password parameter:

    java -Dspring.data.mongodb.password=password -Dspring.data.mongodb.username=username -jar analyzer-server.jar

Option 2: Create an environment variable to store the password:

    export SPRING_DATA_MONGODB_USERNAME=username
    export SPRING_DATA_MONGODB_PASSWORD=password

More information on option 2 at the [spring boot documentation](https://docs.spring.io/spring-boot/docs/1.5.5.RELEASE/reference/html/boot-features-external-config.html)

### Demo installation

In case you are running one of our demos, the required files for the embedded mongodb are automatically downloaded during the first startup. If you have no internet access or if your user has no rights to create the '.embedmongo' folder in your home directory, you can manually copy the files to their target location. Otherwise you can simply move ahead and [run the twx-Analyzer demo](start-server.md).

These are the paths where the embedded mongodb files are expected:

    Windows:   %HOMEPATH%/.embedmongo/win32/mongodb-win32-x86_64-3.5.5.zip
    Linux:     $HOME/.embedmongo/linux/mongodb-linux-x86_64-3.5.5.tgz
    Mac:       $HOME/.embedmongo/osx/mongodb-osx-x86_64-3.5.5.tgz

Downloading the MongoDB file manually:

!!! note "Download links:"
    https://fastdl.mongodb.org/osx/mongodb-osx-x86_64-3.5.5.tgz

    https://fastdl.mongodb.org/win32/mongodb-win32-x86_64-3.5.5.zip

    https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.5.5.tgz


---

# Start Server



## Run twx-Twxanalyzer

If the twx-Analyzer is not configured as a service you can run the server with these commands:
<!-- TODO mehr details -->

Linux:

    1. Start or check if Mongodb is started
    2. Open terminal window
    3. Navigate to the analyzer folder
    4. Start the twx-Analyzer
    `java -jar <analyzer-server-2.x.x>.jar`

Windows:

    1. Start or check if Mongodb is started
    2. Open command prompt
    3. Navigate to the analyzer folder
    4. Start the twx-Analyzer
    `java -jar <analyzer-server-2.x.x>.jar`


If the twx-Analyzer has started successfully, you will find a line similar to this in the log file:
    
!!! success
    INFO  .TwxAnalyzerBackendApplication logStarted : Started TwxAnalyzerBackendApplication in 11.743 seconds (JVM running for 12.908)

!!! tip
    In case you need to debug the servers' startup sequence, you can add a debug parameter to the start command: 
    
    java -Ddebug -jar analyzer-server-2.x.x.jar

## Run demo

During the initial startup the embedded MongoDB is downloaded, if this does fail please check the [section: demo installation](installing.md#demo-installation).
The demo requires a 64-bit Java 8 or higher. Therefore, we provided start scripts for Windows and Linux to check the prerequisites.

Windows:

    * start.cmd

Mac/Linux

    * start.sh

---

# Configuration

There are lots of configuration possibilities.

### Startup Configuration

When starting the application, some parameters can already be changed that influence the application. Roughly the configuration can be divided into 2 parts 1. technical and 2. application related parameters.

#### technical parameters

By default the server starts with a self-signed ssl enabled on port 8443

| Parameter | values | note | 
| :------------ | :-------| :-------- |
| logging.file.max-size | xxxMB | Control of the log file size |
| server.ssl.enabled | true/false | Disable SSL | 
| server.port | 8082 | Default server port e.g 8082 | 
| management.port | 9091 | Default management port e.g 9091 |
| debug | none | Debug Mode enabled whne parameter is set without value |
| server.ssl.key-store-type | PKCS12 | The format used for the keystore. It could be set to JKS in case it is a JKS file |
| server.ssl.key-store | keystore/twxAnalyzer_keystore.p12 | The path to the keystore containing the certificate |
| server.ssl.key-store-password | changeit | The password used to generate the certificate |
| server.ssl.key-alias | twxAnalyzer | The alias mapped to the certificate |

Example:

    java -Dlogging.file.max-size=100MB -Dserver.ssl.enabled=false -Dserver.port=8082 -Ddebug -jar ...

MongodDB

| Parameter | default value | note |
| :------------ | :-------| :-------- |
| spring.data.mongodb.database | twxAnalzyerNew | Change the name of the database where the data is stored |
| spring.data.mongodb.host | localhost | hostname of the mongodb |
| spring.data.mongodb.port | 27017 | post of database |

<!--authentication-database: twxAnalyzernew
username: admin
password: D3veloperPwd-->

System

System configurations are used as database default values at a first startup. Afterwards they have to be changed via the webfrontend.

| Parameter                        | default value                           | note                                                                                                                                                                                                                                                                  |
|:---------------------------------|:----------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| system.analysis.keep-files       | true                                    | Keep uploaded files after analysis                                                                                                                                                                                                                                    |
| system.analysis.isf-features     | false                                   | ISF feature switch                                                                                                                                                                                                                                                    |
| system.analysis.exclude-toolkits | SYSRC,SYSRPC,SYSCM, SYSBPMUI,TWSYS,SYSC | From analysis default excluded toolkits                                                                                                                                                                                                                               |
| system.analysis.remote-download  | false                                   | Enabled the remote download capabilities                                                                                                                                                                                                                              |
| system.analysis.plugins-folder   | /plugins                                | Default location of the plugin folder. If you would like to set a different folder at startup (folder can also be change via frontend) set the following property: -Dsystem.overwrite.plugins.folder=xyz or environment variable: SYSTEM_OVERWRITE_PLUGINS_FOLDER=xyz |
| system.analysis.fulltextsearch   | true                                    | Enables the full text search                                                                                                                                                                                                                                          |
| system.security.cicd             | true                                    | If true security for the cicd endpoints is enabled. If false it can be used without proper login                                                                                                                                                                      |
| system.security.actuator         | true                                    | If true security for all actuator endpoints is enabled, except health and info. If false it can be used without proper login                                                                                                                                          |
| ldap.securityPropertiesPfad      | ''                                      | Default is empty string. It is used to search for the location the security.properties. If not set the file is expected to be at the same folder. In the docker environment the file is placed under root '/'                                                         |


<!-- system.analysis.umlaut-replacement  false  Enabled the remote download capabilities  -->

BAW or ISF server connection settings

| Parameter                        | default value                           | note                                                                                                                                                                                                          |
|:---------------------------------|:----------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| server.map.bpm.hostname          | ibmbaw                                  | hostname of the baw or isf server                                                                                                                                                                             |
| server.map.bpm.port              | 9443                                    | BAW connection port. Most likely 433 for https                                                                                                                                                                |
| server.map.bpm.protocol          | https                                   | Protokoll to choose between http and https                                                                                                                                                                    |
| server.map.bpm.use-webclient     | true                                    | Uses java webclient code to connect                                                                                                                                                                           |
| server.map.bpm.new-download-api  | true                                    | Uses BAWs 'new' api to download twx-files                                                                                                                                                                     |

The BAW/BPM administrative user or password is provided in the HELM Chart via secrets or in case of a jar-deployment as an environmentvariable

Helm-Chart:

```YAML
secrets:
    baw_technical_user: admin
    baw_technical_user_pw: admin
```

Resulting in environment-variables:
BAW_TECHNICAL_USER and BAW_TECHNICAL_USER_PW


### Startup script

If you want to create a start bash/cmd-script you can start here with the example start scripts: [unix shell](./start.sh.txt) or [windows cmd](./start.cmd.txt)

Those scripts are just meant to get you started. Feel free to use, change and advance the script.
 
<!--
TODO Password MongoDB
-->

### Changing the certificate

It is possible to use your own certificates with the twx-Analyzer. For that you need to configure these parameters:

// The format used for the keystore.
server.ssl.key-store-type: PKCS12
// The path to keystore
server.ssl.key-store: keystore/twxAnalyzer_keystore.p12
// The default password for the certificate is
server.ssl.key-store-password: changeit
// The alias
server.ssl.key-alias: twxAnalyzer

#### Server-Certificate for online connection
If you need a connection to BPM for automatic downloads, you also have to include the BPM ProcessCenter certificate into the JVM keystore.

<!-- Import Certificate: https://www.ibm.com/support/knowledgecenter/SSUS84_7.5.2/com.ibm.ram.installguide.doc/topics/t_install_eclipse_https.html
https://azure.microsoft.com/en-us/documentation/articles/java-add-certificate-ca-store/ -->
First export the IBM BPM certificate by opening the processAdmin. Use your browser to view and export the certificate locally. Next you need to import it to your local java installation.

Import:

`cd C:\Program Files\Java\jre1.8.0_51\bin`

`keytool.exe -import -keystore ..\lib\security\cacerts -alias ibmbpm856 -file "<Path>\exportedBPM.crt"`

The default password is: `changeit`

<!-- 
3. hosts datei anpassen IP == IBMBPM
cd C:\Program Files\Java\jre1.8.0_51\bin
keytool.exe -import -keystore ..\lib\security\cacerts -alias ibmbpm856 -file "C:\Users\STSE\Desktop\IBMBPM.crt"
keytool.exe -delete -keystore ..\lib\security\cacerts -alias ibmbpm856 -file E:\tmpstse\IBMBPM.crt -->

Listing all available certificates:
`keytool.exe -list -keystore ..\lib\security\cacerts`

<!-- Export certificate: https://liangjunjiang.medium.com/ldaps-certificate-in-spring-boot-application-and-its-docker-image-cf7785acef8f -->
<!-- https://stackoverflow.com/questions/27724544/specifying-trust-store-information-in-spring-boot-application-properties -->

#### Truststore

If you are planing to connect to the IBM BAW ProcessCenter there are 3 possibilities to solve the certificate issues.
1. Import ([see here](../..#Changing_the_certificate)) your domain certificates into the keystore and override the default keystore.
2. Import the certificate into the java key store, as described in the previous section
3. Import the server certificate into a trust store and add the following parameters to your start command:
`-Djavax.net.ssl.trustStore=..\path_to_cert\filename.crt -Djavax.net.ssl.trustStorePassword=xxxxxxx`

If the target servers such as Active Directory or Process Center are not in the same network as the twx analyzer and the certificates differ as a result, the third variant should be selected. 

### More configuration parameters

Spring boot, on which the application is build on, offers some more configuration alternatives. You can read about those in the [spring documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config). So one alternative is to set environment variables. And for that you can transform the configuration name into a environment variable like:

    -Dserver.port to SERVER_PORT

And then use the variable in your .bashrc

    export SERVER_PORT=8443

A dash will be replaced with no value. Lets have a look at some examples:

    server.ssl.key-store-type     --> SERVER_SSL_KEYSTORETYPE
    server.ssl.key-store-password --> SERVER_SSL_KEYSTOREPASSWORD

<!-- https://github.com/spring-projects/spring-boot/wiki/Relaxed-Binding-2.0 -->

### Shutdown Endpoint

By default the shutdown endpoint is enabled reachable via

    curl -X POST http://localhost:9090/actuator/shutdown

To turn off this capability, start the server with additional configuration:

    -Dmanagement.endpoint.shutdown.enabled=false


<!--
### Login Behavior
There are two options in the twx-Analyzer use LDAP as the user provider or use the possibility to save register new users, which are then saved in the mongoDB.

#### Controll user repository (MongoDB) 
If you decide to go with the mongoDB ask our support for settings.
-->

### Login with LDAP repository

[//]: # (TODO)

#### Overwrite security.properties

Normally the security.properties file is used to configure the Active Directory
or LDAP configuration. Alternatively, the parameters can be passed as arguments when starting the application.

This would look like this:

    java 
        -Dldap.url=awstswn086.twx-solutions.net 
        -Dldap.port=636 
        -Dldap.baseDn=DC=TWX-SOLUTIONS,DC=NET 
        -Dldap.managerDn=CN=TWX2BPM,OU=UsersTechnical,OU=UserAccessService,DC=TWX-SOLUTIONS,DC=NET 
        -Dldap.managerPassword=xxx 
        -Dldap.userSearchBase=OU=UserAccessService 
        -Dldap.userSearchFilter="(cn={0})" 
        -Dldap.sslEnabled=true 
        -Dldap.groupSearchBase= 
        -Dldap.groupSearchFilter= 
        -Dldap.searchControllerEnabled=true
    -jar analyzer-2.1.0.jar

[//]: # (#### Use ////)

##### SSL configuration

Alternatively use the commands and add the certificate to the java keystore

```$ openssl s_client -showcerts -connect ldapserver:636```

The output will contain a number of entries delimited with

-----BEGIN CERTIFICATE-----
aklfhskfadljasdl1340234234ASDSDFSDFSDFSDFSD
....
-----END CERTIFICATE-----

Copy the last certificate entry into a file (ldapca.crt)

Then, add it to the java keystore in $JRE_HOME/lib/security

    $ cd $JRE_HOME/lib/security
    $ keytool -import -alias ldapca_self_sign -keystore cacerts -storepass changeit -file ldapca.crt

Also have a look at the [system settings section](.../components/settings.md#server-certificate-for-online-connection)

### LogLevel Configuration

To change the log level, a rest api (http) endpoint is provided.

Request LogLevel via curl:

    curl -i -k http://localhost:9090/actuator/loggers/de.ssee.analyzer.plugin

Change LogLevel via curl:

    curl -i -X POST -H 'Content-Type: application/json' -d '{"configuredLevel": "TRACE"}'
    
    http://localhost:9090/actuator/loggers/de.ssee.analyzer.plugin

### twx-Analyzer configuration

Please see the ["Settings"-section](../components/settings.md) of the twx-Analyzer the following configurations are available.


---

# Overview

# Overview

<!-- TODO auch bei capabilities auf der welcome page -->
Plugins create the analysis result and deliver so called ’findings’, which indicate that some code
or configuration disobeys a certain rule and therefor a developer should attend to and change
it. But the twx-Analyzer also delivers tools and functionality for other tasks and questions,
that come in handy when developing with IBM BPM. The following sections explain the basic
capabilities.



## [Start](startsection.md)

This section is the tool to manage, upload and analyze snapshots. It is the control center for
snapshots.

* Snapshots: Lists all analysed snapshots and shows the current activated snapshot.

* Upload: You can upload and start analysis of BAW and ISF files.

## [Analyze](analyze.md)

List the findings of the currently selected snapshot in different categories.

## [Develop](developer.md)

There you can find tools which are helpful for the day to day development tasks.

## [Explore](explorer.md)

It contains tools for a better overview into a snapshot and contains search functionalities.

## [Settings](settings.md)

The system and plugin configuration is editable in this section. It also contains the overviews off available plugins.

---

# Startsection

## Snapshots

The snapshot overview is a central piece to manage analyzed snapshots in the twx-Analyzer. It lists all snapshots which have been analyzed in the past and highlights the current activated snapshot.

The context menu in the left row provides additional capabilities such as:

* Delete - Delete the snapshot
* Reanalyze - In case new plugins were introduced or configuration has changed you can reanalyze the snapsot.
* Autoload - To have the twx-Analyzer connect to BAW and check for a newer version.

## Upload

This is where exported snapshots can be uploaded and analyzed. By dropping the file into the green area, the file is automatically uploaded to the twx-Analyzer server. After that it is ready for inspection, the analyze button will be clickable.

An alternative to manually uploading files, is to connect the twx-Analyzer (see section: [Settings](./settings.md)) to your BAW server. This enables the possibility to have the analyzer taking care of downloading and starting the analysis. Users will be notified as soon as the analysis has finished.


---

# Analyze

## Findingtypes
Findingtypes categorize the findings by severity. A Plugin can "deliver" several findingtypes of different categories.
These categories indicate how healthy in terms of clean code a snapshot is and if a deployment of those snapshots is advisable.

Categories:

* Error: They most likely create exceptions during runtime. This is the most critical category.
* Warning: Can be findings, that decrease performance or are violations of the coding guidelines.
* Bad Style: Indicate that some code is not following the basic styling rules or are violations of specific coding guidelines.
* Others: Findings created by plugins that do not fit into a specific category. Such can be: A list of all services of type ’rules’.

Each category consists of a list of possible findingtypes. The numbers in figure 1 indicates how many of those findings occurred in the current snapshot. The below pictures contains a list of all findingtypes of severity error, for a snapshot. Details appear by clicking the chip elements. As a result all findings are listed like in figure 1.

![figure_1](../pictures/Error.png "Figure 1: Error findings")

Services and processes can contain multiple findings of different plugins. For this situation a
service overview page is available, which lists findings of every category. This page also contains
the possibility to search for servicenames or processes. In figure 2 it is the ’By Service’ link in
the upper right corner with the green icon.

![figure_2](../pictures/ByServiceFilter.png "Figure 2: By Service")

<!-- TODO exclude erwaehnen -->
### Exclude services

To exclude services from all or certain findings types, create a tagGroup in BPM named 'twxAnalyzerIgnores' and add tags that carry the name of a findingtype. By adding the tag of this tagGroup to a service, the service will be excluded from the analysis. To ignore all findings at once, you can use the tag 'All'.

## Findings

These are the analysis results and belong to a findingtype. If a finding is found in a toolkit, behind the service name there will be a small ^TK^, which indicates that the source of the finding is a toolkit. Hovering over the short text will show a tooltip containing the name of the toolkit acronym.

### Ignored Findings

As mentioned before you can exclude entire services from the finding result list with a [tagGroup](analyze.md#exclude-services). 
You can also ignore a specific finding. Go to Error/Warning/Bad Style and choose a findingType and then select the finding 
that shall be ignored. 

More details: 
Since the analysis of a ProcessApp cannot distinguish which analyzed service is actually relevant and which, for example, 
was built for a quick "service test" and therefor may contain errors. The goal, of course, would be to avoid receiving findings of this 
nature or other undesirable findings reported as errors again and again in each analysis.

If a finding is not desired and should not be included in subsequent analyses and therefore in new snapshots, 
it can be marked in the frontend after an analysis. When the page is then refreshed, the finding will no longer be displayed, 
and even in a completely new analysis of the current or a new snapshot of this app, the finding will not be reported again.

In other words, the finding will be ignored until the ignore setting is revoked. 
Even a analysis of a newer or older snapshot will not show findings which match the that has been chosen to ignore. For more
information how many snapshots are preserved when new a snaphot is analyzed check the [configuration settings](./settings.md#others). 

An overview of findings marked as "Ignore" can be found at Analysis -> Ignored Findings . However, 
this will also display findings or findings to be ignored that may already be fixed or not available in the current 
snapshot for other reasons.

Even in a CI/CD call, no findings which are to be ignored are provided.

---

# Developer

Contains helper functionality for developers, as well as some tools for deeper analysis of the structure of a snapshot.
It is very important to know which elements and how elements are used in the snapshot or if there are unfinished tasks.

* ServiceNaming: A good practice is to name exceptions uniquely and similar to service where they are possibly thrown.
  Please see [best practice](../bestpractices/all.md). This tool creates the ExceptionEventNames out of the services
  name. If developers decide to follow this rule there is a plugin, that creates findings if an ExceptionEventName does
  not match.


* Field Refs: Certain projects have recurring name patterns, or one needs to find all occurrences of a certain text.
  This is where the system shows all search results for such demands. The search patterns are regular expressions, which
  are configured in the settings section.

<!-- * Loggernames: Fine granular control of logging can come in handy. The analyzer delivers
functions and best practice for a logging approach. If this approach is used, the logger
names are listed in this section. Please see Appendix A ’Appendix’ : for detailed description
and howto.
-->

* Resources: Dealing with localization in large projects can be difficult. In here all available localization variables
  are listed. It is more convenient to search in this tool and look for already available text, then in the process
  designer. The listing contains also shows how many times a certain variables is used in the code.


* Todos: Lists all places where a developer has placed a todo. By adding some keywords to the TODO, the system can even
  recognize for whom the item is created and when it was created. The current expression how a TODO is defined is:

  TODO [ from ] origin_name [ to receiver_name ] date ( DD.MM.YYYY ): text


* Search: This is the first version of a full text search. The search is available for 5 different types of elements [bpd services (process), epv, coach views, objects (twClass)]. In the result table a list of elements are visible. By choosing one the content of that element is presented as a json tree on the right hand side. For more details see below description.  

### Details - Field references

Basically the field references are the first step towards a full text search. Like previously mentioned, it uses regular
expressions to find matches of these expressions. The search will be performed at all places where code, documentation, notes or names can be or are specified. The configuration of the list can be done in the following section:
Settings -> Plugin settings -> "Fieldrefence search variables". To add a new entry to the list
the text field must be filled with a valid regular expression. Afterwards the value
is confirmed with "Enter" and finally transferred to the database by clicking the "Submit" button. Changes to the list, such as
for example, adding or removing an entries, without subsequent saving via the Submit button are not persisted permanently.

Currently it is not checked if the entry is a valid regular expression, in this respect special care has to be taken that the entries are correct.

If an ISF (APPX) file is analyzed with the twx-analyzer, the object/app namespaces are automatically added to this list. This allows to search in the ISF app as well as in the BAW application.

All entries are applied to the twx-analyzer instance for each analysis and for all projects.

It should also be noted that many entries may have a negative impact on performance, so it is advisable to check the list from time to time.

### Details - Full text search

#### Description

The fulltext search will eventually replace the existing search via predefined regular expressions (RegExp search). For the time being, however, both variants will remain.
There are two main differences between the two search options. For the RegExp search, in order to get results, the search parameter must be configured as a regular expression before the analysis. The fulltext search does not need any previous configuration. However, the RegExp search can show an exact path (tags in the XML) to the location of the found values from the elements (e.g. BPD or Service).

Each time a new snapshot is analyzed, the elements of the twx are indexed. It is important to know that the full text search currently only indexes and stores only those element types:
TwClass, BPD, Service, EPV, CoachView.
This restriction is necessary because it is not yet possible to estimate exactly how much storage space indexing will require. Of course, the search results are also limited to these object types.

The fulltext search needs at least a MongoDB in version 5


#### Configuration

The full text search is activated by default. However, it can be disabled at any time via the [configuration](settings.md#general-settings).
Additionally, there is the possibility to change the configuration at the first startup.
A description and list of the startup parameters can be found in the [installation manual](../installation/configuration.md#startup-configuration).

#### Indexing

Indexing is performed with each new analysis of a snapshot and only for the process app. With Release 2.3.3 toolkits are indexed as well. Keep in mind that we, on purposely do not index system toolkits. Those are configured in the system properties. 

With a new analysis the indexes of the previous analysis are deleted. This serves to save space and not to load the system with unnecessary data. The associated indexes are also deleted when a snapshot is deleted. In concrete terms, this means that when analyzing snapshot V1, the indexes for V1 are also created. When analyzing snapshot V2 (same project), the indices of V1 are deleted and newly created for V2. If V2 is now deleted, snapshot V1 is visible again in the snapshot overview, but has no indices for the fulltext search. Without uploading a new snapshot or re-analyzing the existing snapshot V1, no new indexes will be created and an error message may occur. In this case, the current snapshot V1 must be analyzed again.

#### Search

The full text search is located under "Develop -> Search". As everywhere you can use the key combination "ctrl + shift + f" to move the cursor into the text field of the full text search

![figure 1](../pictures/fulltextsearch.png "Figure 1: Full text search header")

Most punctuation characters in a search string are seen as separators. This can be seen well in an ID search if the ID was not surrounded by apostrophes. However, the exception to the punctuation is the hyphen. It is used (unless enclosed in \") to negate a word.

* Stop Words:

  If the search string contains so-called stop words (the, and, ...), these are ignored by the search.


* Word stem:

  When searching without case sensitivity, the complete root of a word is matched. So if a serivce contains the word blueberry, a search for the term "blue" will not be successful. However, blueberry or blueberries will match. 


* Negations:

  - The negated word will cause documents containing this word, to be excluded from the results list.
  - If there is a hyphen between two words without a space before the - (e.g. co-worker) this does not lead to the negation of the word worker.    


* Phrase:

  With this checkbox you can control whether the entered text is used as a unit or coherent text (if selected), or whether the specified words may occur in any order and in any places in a text.
  You can also crate a phrase by wrapping the text with \", but keep in mind that using a phrase and normal text in the search string, the result will only contain the results of which the phrase has matches. With 
```"\"IBM BAW\" service"``` only  ```"\"IBM BAW\""``` is used to create search results.


* Case sensitive:

  If this checkbox is selected, the search will be performed to the letter. The search will then contain only results where the words follow the upper or lower case of the search text. Case sensitive searches may be more performance demanding then non case sensitive searches. 


* Current Project:
  
  To search only in the values of the currently selected snapshot, this checkbox must be selected. Otherwise, the search will be performed over all available snapshots. It should be noted that currently it is not yet shown from which snapshot the respective encounter comes.


* Element list:
  
  This list contains all elements that are currently indexed. Only selected types are shown in the result list. If no checkbox is selected, all element types are included.


* Tk or App Filter:

  This list contains the toolkits and process app which are included in the search result. So you can filter out either results from the process app or any toolkit.


* ID search:
  
  With the full text search it is now easier to search for IDs. However, in order to get exact results, quotation marks should be placed around the ID to be searched.


!!! important "Partial Search"
    MongoDB does not yet support partial search, therefor if the name of a service is: ```ComplexHumanService``` and you type ```HumanService```, there will be no results. There is a feature request for this. Please feel free to upvote it: https://jira.mongodb.org/browse/SERVER-15090  

##### Handling search results

The results of the search are displayed below the search parameter bar.
When selecting a single search result, the JSON string of the object in which the value was found is displayed on the right side. The display can be completely collapsed using the "Toggle" button.
It is also possible to select multiple search results at the same time and thus display all Json strings simultaneously on the right side.


---

# Explorer

This section thrives to give a deeper insight into the snapshot. It contains functionality
like a toolkit hierarchy tree, as well as list of all services.

* **Toolkit Explorer**: Contains a tree representation of the used toolkits.
* **Environment Vars**: It is difficult to get an overview of all environment variables, especially if their toolkits which contain environment variable definitions.
* **System Repository**: Better insight into services or processes, as well as their variables and documentation. This tool also provides a wide range of additional capabilities, such as generating code, which is good practices compliant.
* **Snapshot Diffs**: Compare two snapshots for increment or change identification.
* **Orphan Admin**: Show all orphan toolkits and their state for manual archiving or deletion.

### Details - Snapshot Diffs

This tool offers the possibility to compare snapshots created in the ProcessCenter with each other. The backend server creates a Git repository, 
which is filled with the two snapshots (base and commit). 
The base snapshot goes into the first initial commit and the second snapshot (called: commit snapshot) into the second commit.
Afterwards a standard Git-Diff command is executed with the tooling "jgit". The result is transferred as a unified string to the frontend
which generates a well readable representation.

First we do remove all the XML-files that belong to System Toolkits, that does speed up the calculation and those are not needed in the comparison since
you should never ever make manual changes to these toolkits. But please let us know if you have other requirements.
Then, to make the BAWs (not always valid) XML documents (partly multiple XML declarations) valid, readable and processable each file has to undergo a
cleaning process (called "makeValidXml"). Afterwards certain tags are removed ([tag list configuration](../components/settings.md#snapshot-diff)), then for 
the comparison reasons the XML is stripped of its namespaces, before the server performs character unescaping, and finally sorts certain tags based on their UUIDs.
At last, the XML is turned into a nice form (pretty-print).
These steps are necessary on the one hand to make the XML processable at all and then to get as few wrong differences as possible when creating the Git-Diff.

If two snapshots of different process apps are selected, the system asks if this is really wanted, 
because this can lead to very long calculation times and differences. Such a difference can contain up to 20MB of data.
And of course the more changes between two snapshots the bigger the differences and there this has direct impact to the frontend. 
The unified diff string has to be interpreted into HTML and this can lead to a lot of additional HTML-tags. This has to be kept in 
mind also when a historic snapshot is selected.

It is possible to select the same snapshot for base and commit, but the system will ask if this is really intended.

Currently, a connection to the BAW server is required when selecting snapshots. If this configuration is not active, no comparison can take place,
because the snapshots cannot be downloaded. In addition, the snapshots must be configured to be saved to MongoDB after downloading.
If this is not active, no processing can take place either. Among other things files are required to avoid having to call the BAW server again when comparing snapshots.

Once generated, comparisons are stored in the database and referenced when run again. They are also accessible via the history tab.

In the course of generating the comparison, the files available as <UUID>.xml are renamed to make them easier for 
the user to read. Unfortunately, IBM BAW allows special characters in the names of the services, BPDs, etc.. which are not allowed in Unix or Windows as file names.
For this reason all characters these characters: `<>:"/\|?*` are replaced by an underscore. 
Then there are cases when IBM does not provide a name for the xml files, that's the reason you may see UUIDs in the diff.

!!! note
    Currently, it is not possible to select Toolkits for comparison. In this case you have to wait for manual upload feature.

!!! important "Two snapshot versions in a ProcessApp"
    If you compare a Snapshot that has, in its toolkit-dependency-tree, two different versions of the same Tookit referenced, then 
    unexpected results may appear. The reason is, that the toolkit is extracted in a folder named like the toolkit project name 
    and if there are two versions of the same toolkit, then files may be overwritten and the order is not controllable. For now,
    you need to analyze the snapshot and check with the ```Toolkit Conflict``` plugin.

!!! important "File links"
    Currently the comparison view has a clickable link on the file list, but unfortunately that link is not working. We are trying
    to find a workaround. Be careful with a click it reloads the page and you would have to recall your comparison from the history tab.

#### Elements of the tool

Next to the button to start the comparison there is a checkbox "Sort Tags". Since the sorting takes a lot of time, this gives the user the possibility to switch sorting off.
However, this leads to the fact that the result is sometimes more difficult to read, because the order of the tags in the XML files
makes for more differences that are actually not differences at all (false positives).
It should also be noted that an analysis previously performed with a different "Sort" status must be run again with the changed flag to incorporate this change into the result.

As mentioned, tags are also removed when processing the XML files. These tags usually contain UUIDs, which
often change for unknown reasons and thus create many uninteresting differences. In principle
any tag can be removed with this configuration. The list can be found in the [configuration section](../components/settings.md#snapshot-diff) of the application and
should be customized with care. Also, sorting Tags often needs the UUID of those tags, so we recommend customizing it in coordination with us.

The history view contains all non-sorted comparison requests of the past. In this view you can delete any entry or administer long-running and
potentially never finished request. If such request takes longer then 30 minutes a button occurs which allows you to mark the comparison as
finished.
By double-clicking on a row the view switches to the other tab a displays the selected results.

#### Diff UI

The configuration or filter values for the diff-ui are stored in the local browser storage. Therefor changes
are saved until the browser is reset. It is a temporary saving mechanism and has no effect for other users.

To control how results are presented you have some options. 
* Output format: choose between inline or side by side view. Default value is inline (line by line).
* File summary: The list and overview on the files that change can be toggled with this checkbox. The default value is true.
* Matching type: Matching level: 'lines' for matching lines, 'words' for matching lines and words or 'none', default is none
* Max. line length for highlighting: A difference may contain multiple differences even on the same line, determined by
  the number of characters in a line, if this threshold is exceeded, the differences per line will not be highlighted. 
  The default value is 100. 
* Max. line length to display: A difference may encompass multiple lines, determined by the number
  of characters in the longest line, if this threshold is exceeded, it would conceal the diff.
  Remember: longest line is taking precedence. The default value is 150.
* Max. changes to display: A difference may encompass multiple changes, determined by the number
  of changes, if this threshold is exceeded, it would conceal the diff. The default value is 120.

Other options can be included if necessary.

As mentioned before the UI has an issue with the click on a filename, that leads to a page reload.

Besides that, your viewed files are not stored in database, meaning if you reload the page those checkboxes are unmarked.

Also keep in mind that IBM sometimes has no name for the xml files. That is especially the case for SubProcesses, 
which leads to an ID as filename instead of a readable and  allocatable entry.


#### Performance

We are aware that storing the data on the file system is not yet an optimal solution and can be a bottleneck for performance.  
We will keep introducing optimizations here. But also for this reason we recommend not to run more than one or two comparisons at a time as this will affect performance.The frontend has a display or infobox that shows, if an analysis is already running. 
An analysis for two identical snapshots cannot be not be run twice.

Also the frontend creates a lot of HTML-tags therefor depending on your computer and the amount of differences, this 
could take some time. 

#### Further development

The aforementioned manual upload will be provided in subsequent versions.

The sorting of objects puts a heavy load on processing, so we were initially very conservative with the list
of tags to be sorted. However, extensions can be made at any time.

#### Troubleshooting

###### MongoDB Exceptions

Should exceptions of this kind occur:
 `org.bson.BsonMaximumSizeExceededException: Payload document size is larger than maximum of 16793600.`
Thus, due to the size of the results, the default configuration of MongoDB must be adjusted.
In this case MongoDB tries to store data larger than 16MB.


### Detail - Orphan Admin:

This tool displays the list of all orphan Toolkit snapshots or even Toolkits in the configured IBM BAW server. 
You have to have a connection to your BAW server configured. You can do this by enabling remote downloads ([settings](./settings.md#server-configuration)). 

In release 2.3.1 it is not yet possible to archive or delete the snapshots in this tool. It has to be done manually, but this at least
gives you the information which snapshots could be archived or deleted.
To delete an orphan toolkit snapshot, it must first be archived. If a snapshot is orphan an already archived, so ready for deletion,
it is shown by a tag behind the snapshot name.

---

# Settings

This section holds the system settings and plugin overview section. For more detailed information on configuring the system, please check out the [configuration documentation](../installation/configuration.md)

## System

### General settings

#### System Settings

##### Server configuration

* Enables remote download: You can download snapshots from Process Center via frontend
* BPM hostname: Hostname of the process center

##### BPM credentials

* User: User with administrative rights to call APIs and download Snapshots of any project.
* Password: Password for the user
* Activate download via WebClient-Infrastruture: Do not change, this is the new default and faster approach for downloading files
* Activate download via new BAW-API: Download of twx-Files will happen via the new BAW-API. ATTENTION: This feature will only work if WebClient-Usage is enabled!


##### Plugin settings

* Plugin folder: This is the folder where all plugins are located
* BPM hostname: Hostname of the process center
* Protocol: You should always use HTTPS
* Port: HTTPS Port of the ProcessCenter

##### Snapshot Diff

This is a list of Tags or Nodes that will be removed from XML and therefor not be part of the diff. 
You can use XPATH to address new tags, nodes or even attribute. For example this ```inputPort/@id``` will remove the 
id-attribute from the inputPort node. An this would remove the id-tag below layoutItem ```layoutItem/id```. 

The default list is:
  - lastModified
  - lastModifiedBy
  - modificationDate
  - versionId
  - guid
  - jsonData
  - parameterMappingId
  - parameterMappingParentId
  - bpdParameterId
  - tWComponentId
  - bpdEventId
  - faultParameterId
  - switchConditionId
  - endStateId
  - outputPort/@id
  - inputPort/@id
  - defaultInstanceUI/@id
  - ownerTeamInstanceUI/@id
  - expression/@id
  - condition/@id
  - inputPort/@id
  - outputPort/@id
  - assignment/@id
  - inputSet/@id
  - outputSet/@id
  - laneFilter/@id
  - htmlHeaderTag/@id
  - formTask/@id
  - teamFilter/@id
  - EventAction/@id
  - EventActionImplementation/@id
  - inputActivityParameterMapping/@id
  - extension/@id
  - coachEventBinding/@id
  - artifactReference/@id
  - nameValuePair/@id
  - layoutItem/id
  - configData/id
  - contentBoxContrib/id

##### Others

* Keep snapshot files [Default: true] : Snapshots are stored in the database after download.
* Activate fulltext search [Default: true]: Toggle for the fulltext search feature.
* Snapshot History [Default: 2]: The amount of snapshots which are kept when a new analysis for this ProcessApp (and Track) will be performed.

As a result of the Download Service revamp and improvement, there are now two new configuration parameters in the backend:
- Activate download via WebClient-Infrastructure: determines whether new Download Service classes should be used (true) or the old implementation (false). 
- Activate download via new Baw-Api: determines whether the new Download API should be used (true) or the old one (false). (This parameter only has an effect when 'Activate download via WebClient-Infrastructure' is set to true, as only the new Download implementation supports the new Download API.)

If you need to setup certificates in order to get connection to the process center, navigate to [configuarion](../installation/configuration.md).

#### Plugin settings

Every plugin may define a set of configurations, this is where these configuration are collected.

#### Theme settings

You can change your theme here

## Plugins

In this section it is possible to configure which toolkits are used for which process app. First you need to choose a process app.

![Select process app](../pictures/plugin_select_processapp.png)

Only process apps, of which a snapshot is already analyzed can be selected.

The rows are devided into three colors, which match the colors of error, warning and bad style findings.
In the header row and first column, the green plus icon allows to add toolkits which are part of the selected process app. Afterwards you can choose which findingType shall be applied to the toolkit.

![Fine grained findingtype configuration](../pictures/plugin_toolkitselection.png)

---

# Overview

# Overview standard plugins

Plugins are created to find errors, warnings, bad style implementations. 

Basically there are two types of plugins. Those which are tightly bundled with the product and those which were customized to your project needs. Below topics cover all currently available plugins, that are included in the standard installation. A list of plugins, which are available in your current twx-Analyzer setup, is accessible at the settings tab in subsection "plugins" of the twx-Analyzer.

One of the main aspects of the twx-Analyzer is that projects or institutions can develop their own plugins. Please contant us if you plan to do so. <!--[develop their own plugins](../plugindevelopment/overview.md).-->

## [Error Plugins](details/error-plugins.md)

Most error findings would result in a runtime error.

## [Warning Plugins](details/warning-plugins.md)

Warnings may be performance related or could be cases, that do not follow your [good practices](../bestpractices/all.md).

## [Bad Style Plugins](details/badstyle-plugins.md)

This simply contains findings, which do not follow certain style guides or common agreements.

## Other Plugins

These types of plugins are informational list, which do not necessarily point towards a problem. They could be used to extract certain recurring information. So far none of these plugins are available with the standard delivery.


## More information
### Ignore findings
In case of 'false positives' or for some reason the results of a specific finding type should be ignored, the analyzer uses tagging to accomplish this task. Simply add a tag group named "TwxAnalyzerIgnores" to your project and inside that tag group use the title of the plugin as the name for the tag. Set this tag to an object and the results will not be generated for this.

I case an element should always be ignored, provide the tag 'all' inside the tag group.



---

# Error Plugins

### Next Service Item Not Defined
Description:

The outgoing flow of a service/gateway/script/.. does not flow to the next service. This will result in a next service item not found exception once this flow is reached. This error will only be thrown during execution.

Supported item types:
Service, BPD

Example:

GSS NextServiceItemNotFound |
------------ |
The service ’GSS NextServiceItemNotFound’ has potential Next Service Not found at item: ’Switch - No Next Service Item’. |

### Unattached End Events
Description:

Unreachable end events should not be present in a service. They clutter the diagram and make using the service as a sub service more complicate, since it is not clear on that level which end
events are usable and which ones not.

Supported item types:
Service, BPD

Example:

NotWiredEndEvents Part2 |
------------ |
In the service ’NotWiredEndEvents Part2’ the item ’NotWiredEndEvents Part1’ has ’1’ unused end event. |

### Exception Code Mismatch

Description:

The list of errors that the sub service is able to throw does not contain the error code that the service expects. Each error end event of a sub service has a specific error code. If a service uses this sub service, it has to catch one of these specific error codes.

Supported item types:
Service

Example:

ExceptionCodeMismatch\_Part2 |
----------- |
The item 'ExceptionCodeMismatch\_Part1' has an Error Intermediate Event (catch specific) that's configured error code is: 'EXCEPTION\_CODE\_FEHLER', but the called SubService does not offer such a ExceptionEndEvent. The subServices error codes are: EXCEPTION\_CODE\_MISMATCH\_PART1\_FEHLER |

### Exception Type Mismatch

Description:

The error type of the service and its sub service have to match. Otherwise, the error can not be caught, which will result in a different error from the one that the service tries to catch.

Supported item types:
Service

Example:

ExceptionTypeMismatch\_Part2 |
------------ |
The item: ExceptionTypeMismatch\_Part1 has an Error Intermediate Event (catch specific) that's error mapping is different from the called subService. Current classId cb6b5b0a-f62c-4531-900e-8933b262ddaa/12.db884a3c-c533-44b7-bb2d-47bec8ad4022 and the classId of the subService:12.ec20f773-9bc5-4d3b-a528-7f13793e09f8. |

### Switch Condition Not Implemented

Description:

Every outgoing path of a switch condition, except the default, needs an implementation. Otherwise, the execution creates an exception.

Supported item types:
Service

Example:

Switch Condition Not Implemented |
------------ |
The switch element with name Switch has an output lane that is implemented. |

### Service Not Correctly Attached

Description:

The service references a service that no longer exists or has been renamed. This can happen if a toolkit is removed or renamed.

Supported item types:
Service, UNDERCOVERAGENT, EPV, PARTICIPANT, CLASS, BPD

GSS\_ServiceNotCorrectlyAttached |
------------ |
The attached service could not be found at item: 'PS Get Task URL'. |

### Exception Event Is catch all

Description

Regular services should not catch all errors, since error handling should be handled in the topmost layer. Only proxies and processes should contain a catch all intermediate error events. According to the logging conventions, each service should only throw an error of type "isWebserviceException", which should be caught by the next layer and propagated this way. Since each layer adds its service name, the sequence of called services can be reconstructed that way. This helps in tracing back errors to the service itself by only looking at the error shown in the default error handler.

Supported item types:
Service

Example:

ServiceContainsCatchAll\_Part2 |
------------ |
The ErrorCode (IS EMPTY!!) of the items (item: ServiceContainsCatchAll\_Part1) Exception event is catch All. Exception should be named: SERVICE\_CONTAINS\_CATCH\_ALL\_PART2\_FEHLER |

### Resource Not Linked

Description:

Unlinked resource variables produces exceptions during runtime and are to be avoided!

Supported item types:
Service, BPD

Example:

UndefinedResourceVariable |
------------ |
Found a resource variable that is not linked at service 'UndefinedResourceVariable > tw.resource.Constants.processNotDefined' |

### Next Service Item Not Defined

Description:

The outgoing flow of a service/gateway/script/.. does not flow to the next service. This will result in a next service item not found exception once this flow is reached. This error will only be thrown during execution.

Supported item types:
Service, BPD

Example:

GSS\_NextServiceItemNotFound ||
------------ |
The service 'GSS\_NextServiceItemNotFound' has potential Next Service Not found at item: 'Switch - No Next Service Item'.

### Exception Type Mismatch

Description:

The error type of the service and its sub service have to match. Otherwise, the error can not be caught, which will result in a different error from the one that the service tries to catch.

Supported item types:
Service

Example:

ExceptionTypeMismatch\_Part2 |
------------ |
The item: ExceptionTypeMismatch\_Part1 has an Error Intermediate Event (catch specific) that's error mapping is different from the called subService. Current classId cb6b5b0a-f62c-4531-900e-8933b262ddaa/12.db884a3c-c533-44b7-bb2d-47bec8ad4022 and the classId of the subService: /12.ec20f773-9bc5-4d3b-a528-7f13793e09f8.

### Uncatched Exception

Description:

According to the logging conventions, errors thrown by sub services should be caught by the next layer and propagated up. Each service in this chain appends its service name so that the call sequence can be reconstructed. If one service breaks this chain, the traceability of errors is impacted.

Supported item types:
Service

Example:

UncatchedException\_Part2 |
------------ |
The SubService: UncatchedException\_Part1 has an Exception Event \[UNCATCHED\_EXCEPTION\_PART1\_FEHLER\], that is not catched.

### Unbound Error Variable

Description:

This means the exception handler has no variable bound to it. Therefore, it can not catch the exception from the attached service. This will result in a different error from the original one, making a deliberate error handling impossible.

Supported item types:
Service

Example:

UnboundErrorVariable\_Part2 |
------------ |
The Exception Object of the exception handler at item 'UnboundErrorVariable\_Part1' is empty.

### Unattached End Events

Description:

Unreachable end events should not be present in a service. They clutter the diagram and make using the service as a sub service more complicate, since it is not clear on that level which end events are usable and which ones not.

Supported item types:
Service, BPD

Example:

NotWiredEndEvents\_Part2} |
------------ |
In the service 'NotWiredEndEvents\_Part2' the item 'NotWiredEndEvents\_Part1' has '1' unused end event. |

### Undefined Resource Variable

Description:

A resource variable that has not been defined in the resource file is used. Resource variables have to be defined before they are used.

Supported item types:
Service, BPD

Example:

ResourceNotLinked |
------------ |
Found a resource variable (ResourceUnlinked.IamNotExisting) that is not defined at item 'Script:ResourceVar' in Path: '/item\{Script\}\[ResourceVar\]/twComponent/script' and resource : ResourceUnlinked. |

### Undefined Variable

Description:

A variable that has not been defined is used. The plugin does not recognize if variables are in a comment, it would be considered as dead code anyway. Please keep in mind, that in some cases the missing variable occurred as invisible background variables, especially if the item is connected to UCAs. You might not always see the attribute. Please keep in mind, that this plugin, at this stage does only support the first variable level.

Supported item types:
Service, BPD

Example:

NoEntryLoggingDebug\_Typos |
------------ |
Found a variable that is not defined at item 'tw.local.localLoggerName' in Path: NoEntryLoggingDebug\_Typos:

### UCA Condition Without 'return true'

Description:

In order to function correctly conditions of a UCA should usually return 'true'. This value is needed due to the logging in the condition tab (See good practices : [UCA logging](../../bestpractices/all.md)). If the last line does not return a true statement, there might be other reasons. But usually the line should contain: "true;".

Supported item types:
BPD

Example:

UcaBrokenBpd\_NoCorrelationMethodSelected |
------------ |
Missing return value 'true' at UCA condition in process UcaBrokenBpd_NoCorrelationMethodSelected of item: cancelWithEvent |

### Toolkit Conflict

Description:

Toolkit conflicts are to be avoided, it means there are at least two different versions of the same toolkit imported in your project.

Example:

OftenChangedToolkit |
------------ |
These toolkits or process apps import snapshots of the same project in multiple versions. |
V0.0.1_OCT used by the following snapshots: V02_ADT in AnotherDependencyToolkit |
V0.0.2_OCT used by the following snapshots: 120419_26_TWXBAPP in TwxAnalyzer Beispiel App |

### Class Name Duplicate

Description:

These conflicts may occur if you use service discovery from within the BAW Server. They also appear if two toolkits of
the same origin in different versions are imported in the toolkit hierarchie. Those duplicated objects will be listed. 
This plugin ignores duplicates deriving from system toolkits.

Example:

BusinessObjectSimilarty1 & BusinessObjectSimilarty1 |
------------ |
These two classes have very similar names that might lead to errors.The class names 'BusinessObjectSimilarty1' & 'BusinessObjectSimilarty1' are very similar. Please check if this is intended. The classIds:<br>for BusinessObjectSimilarty1 : '12.f51a9d5e-72f9-4136-94c8-65ab64d07b1b'<br>for BusinessObjectSimilarty1 : '12.51f90bd6-444a-4a98-94ba-b478e484e260'. |

### Exception handling setup error

Description: 

There are different ways to handle exception in BPDs. This is one possibility to handle those. Therefore, this plugin is a project specific approach.
Almost every element in a bpd which must catch exceptions from the called service or subprocess with a catch all, following 
a message send object, with a specific uca. The "Exception token" then waits at a message receive an intermediate event. 
If the event occurs the process is continued. This plugin checks if the catch element is configured correctly, 
checks the needed post assignments as well as correlation parameters. Basically every step of the exception setup.

The catch all element awaits a post assignment which can be configured in the plugin settings.

The sending intermediate event needs an uca as well as an identification variable as input variable.

The message receive event also needs an uca and a set correlation parameter.

![Exception handling setup error](../../pictures/exception_handling_setup_error.png)

If the setup is not correct the following messages might occur:

Message | Explanation |
------------ | ---- | 
Exceptions are not caught at element {%s}. | The element hat no catch boundary event at all. |
Element {%s} has no configured UCA. | The sender or receiver is missing an attached UCA. |
The post assignment of the catch all event of element {%s} seems to be incorrect. | The post assignment seems to be wrong. |
Catch event at element {%s} is missing the necessary post assignment. | The post assignment needs an assignment and a script call. Check the plugins settings page under : "Exception handling" |
The configured UCA at element {%s} has no mapping parameters. | The UCA expects one input parameter with a specific value. Check plugins setting at "Exception handling" -> "UCA parameter correlation name" |
Element {%s} has multiple catch all events. | There can only be one catch all |
Exceptions are not caught with \"catch all\" at element {%s}. Check if it is set to check specific. | The catch boundary event is not of type catch all. It might be a catch specific. |
Element {%s} is not of type UCA send. | After the catch all boundary event the flow needs to go to a intermediate message send event. | 
Element {%s} is not of type UCA receive. | After the intermediate message send event the flow needs to got to a intermediate message receive event. |
The correlation parameter of element {%s} value contains the wrong value. Expected value is: {%s} | The UCA expects a specific variable as the correlation parameter. |


---

# Warning Plugins

### Exception End Event Name

Description:

Exception events must be named to identify the service where the exception occurred. They are named via splitting the service name at the upper letters, chaining each part in capital letters. These characters \"_\" or \"-\" are considered connectors, they are removed and the two connected strings are combined. Then finally adding _FEHLER to the string.

Supported item types:
Service

LowerLetterInExceptionEvent	|
------------ |
Please see item Fehler at Service LowerLetterInExceptionEvent. The Exception Eventname is LOWER_LETTER_IN_EXCEPTION_event_FEHLER, but should be LOWER_LETTER_IN_EXCEPTION_EVENT_FEHLER |

### Localization Resource As Default Value

Description:

Using localization resources at a default value may cause exceptions. When using this service the default must be overwritten or the same localization variable has to be included in the 'calling' service.

Supported item types:
Service, BPD

Example:

DefaultValueWithLocalization |
------------ |
The service DefaultValueWithLocalization uses a localization variable containTitleWithLocalization as default value 'tw.resource.SimpleLocalization.title' |

### Proxy Without Error Information Service

Description:

According to the logging conventions, errors thrown by sub services should be caught by the next layer and propagated up. The proxy is the topmost layer and should present an error message to the logged in user. This service is named 'ErrorInformationService' and should be present in every proxy. Errors thrown by the services should flow to this error handler, as well as errors that are caught by a catch all.

Supported item types:
Service

Example:

defaultCreateProxy |
------------ |	
The service: 'defaultCreateProxy' is a proxy service and might not be able to show exceptions to the user |

### Resource Not Available

Description:

There is a resource variable in the code that's resource file is not attached to the service. Using such resource will results in an error at runtime.

Supported item types:
Service, BPD

Example:

UndefinedResourceVariable	|
------------ |
Found a linked resource (with id: f35d8ae0-1e20-42d4-a3ce-0d93324c0c20/50.3db4cf89-08a4-46e1-8128-69a48db46cc1) that is not available in the process app. |

### Save Execution Context

Description:

A script in this service saves its data upon completion which can have a very big impact on performance. This is only useful in very few cases and often enabled by mistake.

Supported item types:
Service

Example:

SaveExecutionContext	|
------------ |
There is a Save Execution Context setting in service: 'SaveExecutionContext' --> : 'SaveExecutionContext_Item'. |

### UCA Input != Output Parameter

Description:

Input variables are not identical that may lead to a loss of data, cause the uca does send 'null' for uninitialized variables.

Supported item types:
UNDERCOVERAGENT

Example:

ConditionalLoggingWhereUsed_UCA	|
------------ |
Service contains an UCA service, that has different input and output variables. See: SetChange{ 'set' collection changes :
. 'name' added
. 'value' added } |

### UCA Missing Logging At Condition

Description:

The logging conventions require a logging statement at the uca's that receiving the message. This helps to identify when and where a UCA is received.

Supported item types:
BPD

Example:

UcaBrokenBpd_NoCorrelationMethodSelected	|
------------ |
Missing condition logging at UCA in process UcaBrokenBpd_NoCorrelationMethodSelected of item: cancelWithEvent |

### UCA Targeting Snapshot

Description:

The UCA is missing a setting. This UCA is broadcasted into every snapshot and not limited to the one that is currently executed. This might result in unforeseen errors.

Supported item types:
Service

Example:

UcaNotTargetingSnapshotSender	|
------------ |
The UCA NotTargetingSnapshot is missing the 'target snapshot' setting in the implementation tab. |

### Unimplemented Service

Description:

The service has no implementation at all. Services like these are often used during early phases in a project to outline the overall process but should not be present in production code.

Supported item types:
Service

Example:

TestAjaxService	|
------------ |
The service 'TestAjaxService' is unimplemented and has no items. |

### Variable 'serviceName' Does Not Exist (needed prior to BAW 19)

Description:

According to the logging conventions, a variable with the name 'serviceName' has to be present in the service. This variable is used to add the name of the current service to the list of errors from the sub services. This creates a chain of service names, making it possible to trace errors to the service that caused it.

Supported item types:
Service

Example:

ClientSideHumanServiceTest|
------------ |
The service ClientSideHumanServiceTest has no local variable named servicename. |

### Variable 'serviceName' Without Default (needed prior to BAW 19)

Description:

According to the logging conventions, the variable 'serviceName' has to be named the same as the service itself. This should be done during the first script item in the process. An assignment of the services name to this variable is required. This variable is used to add the name of the current service to the list of errors from the sub services. This creates a chain of service names, making it possible to trace errors to the service that caused it.

Supported item types:
Service

Example:

VariableServiceNameWithoutDefault	|
------------ |
The service: 'VariableServiceNameWithoutDefault' has a local variable serviceName, that's not initialized in the first script item. |

### Variable 'serviceName' Has Wrong Name (needed prior to BAW 19)

Description:

According to the logging conventions, the variable 'serviceName' has to be named the same as the service itself. This variable is used to add the name of the current service to the list of errors from the sub services. This creates a chain of service names, making it possible to trace errors to the service that caused it.

Supported item types:
Service

Example:

UcaBrokenBpd_IntermediateService |
------------ |
The service: 'UcaBrokenBpd_IntermediateService' has a local variable serviceName, that's default (UsageOfLogDebug) is different of the services name. |

### Unused Variable

Description:

While developing there can be a whole lot of variable that are never used inside the service.
Those can be local variables of input/output variables. Neither are good coding styles.

Supported item types:
Service, BPD

Example:

UnusedVariable |
------------ |
The following variables are not used in this service: [tw.local.serviceException] |

### Step Name Differs From Service Name

Description:

When a step is named differently from the service where it is used as service in the item flow,
then finding errors that will become more difficult, since exceptions always mention the service
name and not the item name, where that service is used.

Supported item types:
Service

Example:

StepnameDiffersFromServiceName Part2 |
------------ |
The called SubService has a different name then the one set as step name. It should be named: ’StepnameDiffersFromServiceName Part1’ but is named: ’StepnameDiffersFromServiceName Part2’. |

### Proxy Without "Catch All"

Description:

In accordance with the logging conventions, errors should be handled in the topmost service. This means, that every proxy should catch individual errors from sub services and should also contain a catch all, to handle errors that are not propagated correctly.

Supported item types:
Service

Example:

TestServiceProxy |
------------ |
he service: 'TestServiceProxy' is a Proxy service and has no CatchAll Element. |

### Usage Of Old Log-Function

Description:

The log level for services when writing trace information should be 'finer'. Higher levels should be reserved for more critical warnings and errors.

Supported item types:
Service

Example:

UsageOfLogInfo |
------------ |
The service: 'UsageOfLogInfo' uses a log.info in item: 'item:Script:uses log.info' at element: service[UsageOfLogInfo]/item{Script}[uses log.info]/twComponent/script |

### Missing Element Documentation

Description:

The service or BPD is missing its documentation. According to good practices, services and BPDs are supposed to have an explaining text.

Supported item types:
Service, BPD

Example:

GSS_ModifyTaskImplementation |
------------ |
This element is missing its documentation |

---

# Badstyle Plugins


### Switch Output Not Named

Description:

The output lanes of switch conditions are easily mixed and therefore implementation accidentally ends up in the wrong output. To show what lane flows to which item, the lines need an identifying name.

Supported item types:
Service

Example:

SwitchOutputNotNamed |
------------ |
The switch element with name Switch has an output lane that is not named. |

### Element With Default Name

Description:

When scripts and other items are inserted into the service or process diagram, they are named 'default/untitled/...'. This initial name should be changed in order to make the service more expressive.

Supported item types:
Service, BPD

Example:

ElementWithDefaultName_IS |
------------ |
Service contains an item of type: 'Script' that has 'Untitled' in the name. See service: 'ElementWithDefaultName_IS' --> : 'Untitled1'. |

### No Entry Logging

Description:

At the start of a service, trace information should be written to 'logger.finer'. According to the logging conventions, all input variables should be logged at this stage to 'logger.finer' and the first element in a service contains that log statement, the element must be named: 'start logging'.

Supported item types:
Service

Example:

ElementWithDefaultName_IS |
------------ |
The first item Untitled1 of the service seems to be no logging element. |

### No Entry Logging Debug

Description:

At the start of a service, debug information should be written to 'logger.debug'. According to the logging conventions, all input variables should be logged at this stage to 'logger.debug'. Example for a correct Entry Logging:
``` javascript
if(logger.isDebug(tw.local.localLoggerName)) {
    var logMessage = "InputWerte: "; 
    logMessage += "InputVariable1: " + tw.local.inputVariable1;
    logger.debug(logMessage,tw.local.localLoggerName,tw.local.serviceName);
}
```

Supported item types:
Service

Example:

NoEntryLoggingDebug_Typos |
------------ |
The first item start logging of the service seems to lack of debug.logging. |

### No Entry Logging Finer

Description:

At the start of a service, trace information should be provided. According to the logging conventions, all input variables should be logged at this stage to 'logger.finer'.

Supported item types:
Service

Example:

NoEntryLoggingFiner_Missing	|
------------ |
The first item start logging of the service is not properly logging. Use logger.finer(). |

### Switch Output Not Named

Description:

The output lanes of switch conditions are easily mixed and therefore implementation accidentally ends up in the wrong output. To show what line flows to which item, the lines need an identifying name.

Supported item types:
Service

Example:

SwitchOutputNotNamed |
------------ |
The switch element with name 'Switch' has an output lane that is not named. |

### Undocumented Default Variable

Description:

The purpose of default values for variables should be explained.

Supported item types:
Service, BPD

Example:

ProcessWithDefaultIssues |
------------ |
The service ProcessWithDefaultIssues has a variable 'varWithDefaultAndNoDescription' with a default value '"whereIsMyDescription?"' and no description. |

### Variable Documentation Error

Description:

Input and output variables of BPDs and services need documentation. If a variable is 
used for input and output, the input documentation is sufficient.

Supported item types:
Service, BPD

Example:

ProcessWithDefaultIssues |
------------ |
The input variable named "bpdOutputVariable" has no documentation. |

### Incorrect History Entry

Description:

According to good practices the editor of a service or BPD, should update the documentation with a hint of what has been changed.
For that the user shall insert a new line starting with a date.

Example : "dd.MM.yyyy - username - describing text".

Services or BPDs are ignored if they have a UUID as their name, which means they mostly are by the system generated elements. With this Finding, three different message may appear:
* The latest DATE (%s) from history-documentation and last edit DATE (%s) are not equal.
* The latest EDITOR (%s) from history-documentation and last change EDITOR (%s) are not equal.
* The last CHANGE DATE from history-documentation could not be found. Or the history listing is not yet started.

Supported item types:
Service, BPD

Examples:

GSS_SearchableReferenceUsage |
------------ |
The last CHANGE DATE from history-documentation could not be found. Or the history listing is not yet started. |

DecisionServiceSimple |
------|
The latest EDITOR (admin) from history-documentation and last change EDITOR (03.01.2021 - eh2setw - Do...) are not equal. |

ComplexHumanService_migrated |
------------ |
The latest DATE (20.01.2020) from history-documentation and last edit DATE (03.01.2021) are not equal. |


---

# Overview

# Overview of Plugin implementation

To create a new plugin register an account with download.twx-analyzer.de. There you can download a sample project to create a new plugin project.

The new project will contain a lib-folder in which the latest "Development Kit" (analyzer-SDK.jar) resides. This lib is only needed during development and should not be packaged with the new plugin jar.

The next step is to decide which ElementTypes your plugin can handle and anotate the Class with the correct elements:

```Java
@PluginTarget(type = {ObjectType.PROCESS, ObjectType.BPD})
```

Each plugin can create several so called [findingTypes](../../components/analyze.md#Findingtypes). At first these findingsTypes need to be defined. Use the FindingTypeBuilder in class FindingType. In the end, add the new FindingType to the list of all findingTypes via the method 'getLstFindingTypen()'.


This is an example:
```Java
// SETUP YOUR NEW FINDING TYPE
FindingType findingTyp =
    new FindingType.FindingTypeBuilder()
        .setLongDescription(
            "Description of the FindingType so that users known what issue is displayed in the below list of findings.")
        .setTitle("Readable Name Sample") // This is the readable name of the finding.
        .setFindingTypeId(SAMPLE_PLUGIN_FINDINGTYPEID)
        .setShortName(SAMPLE_PLUGIN_NAME)
        .setSeverity(Severity.ERROR)
        .setShortDescription(
            "This is the description that will be displayed if the power users mode is enabled.")
        .setMessageTemplate(
            "The item {ITEM} has an save execution context.") //
        .setHeaderImageUrl(ConstantsHeaderImageUrl.ITEMHASSAVEEXECUTIONCONTEXT)
        .setSolution(
            "Set a default value for the environment variable or to be more precise set a value for each target stage.")
        .buildFindingType();

getLstFindingTypen().add(findingTyp);
```

The analyzer runs each plugin on each element type, if the ObjectTypes (Check the enum class for the availabe different types) match. So the analyzer runs the method:
```java
List<Finding> performAnalysisGetFindings(String snapshotId);
```
on each turn.

Before the performAnalysisGetFindings method is called the initialize method is executed. Within this method the variable singleFileRepresentationElement is set. This is important since the variable is of type BaseTypeFileRepresentationInterface and may contain all sorts of classes which inherit from this base class. Each element type like bpd or process or uca inherit from this base class. So, in order to find out which element the current run is handling, check with java instanceof.

```Java
if (singleFileRepresentationElement
        instanceof ProcessTypeFileRepresentationInterface) { // Is ProcessType (Service)
```

Now you can start creating new plugins.

If the plugin is finished, upload it to the plugin folder and restart the server. The correct folder can be found in the [configuration](../../components/settings.md#general-settings) section.

## Plugin configuration

I case the plugin needs changeable settings, you can do this as well. In the sample plugin project, there is a sample.

```Java
public List<ConfigItem> getPluginDefaultConfigItems() {

    final String GROUP_NAME = "myOwnPluginGroup";
    if (configItems == null) configItems = new ArrayList<>();

    configItems.add(ConfigItem.builder()
            .name("uniqueName")
            .type(ConfigFrontendElementType.TEXT_TYPE)
            .valueText(config.getBasePlugin().getLoggernameVariableRegexp())
            .pluginClassName(this.getClass().getSimpleName())
            .findingTypeId(findingTypeId)
            .label("Regular expression for identifying the loggername content")
            .placeholder("Default: " + config.getBasePlugin().getLoggernameVariableRegexp())
            .groupBy(GROUP_NAME)
            .build());

    log.debug("Leaving with config: {}", configItems);
    return configItems;
}
```

Check the javadoc for further information

---

# Header_Pictures

# Header Files


Code name: 'headerSaveExecutionContext':

![[Material for MkDocs]](../../pictures/header_files/headerSaveExecutionContext.png)

Code name: 'headerCatchAll':

![[Material for MkDocs]](../../pictures/header_files/headerCatchAll.png)

Code name: 'headerServiceNotAttached':

![[Material for MkDocs]](../../pictures/header_files/headerServiceNotAttached.png)

Code name: 'headerExceptionCodeMismatch':

![[Material for MkDocs]](../../pictures/header_files/headerExceptionCodeMismatch.png)  

Code name: 'headerStraightThrough':

![[Material for MkDocs]](../../pictures/header_files/headerStraightThrough.png)

Code name: 'headerExceptionTypeMismatch':

![[Material for MkDocs]](../../pictures/header_files/headerExceptionTypeMismatch.png)

Code name: 'headerNextServiceItem':

![[Material for MkDocs]](../../pictures/header_files/headerNextServiceItem.png)

Code name: 'headerUnattachedExit':

![[Material for MkDocs]](../../pictures/header_files/headerUnattachedExit.png)

Code name: 'headerUnboundErrorVariable':

![[Material for MkDocs]](../../pictures/header_files/headerUnboundErrorVariable.png)

Code name: 'headerNoEntryLogging':

![[Material for MkDocs]](../../pictures/header_files/headerNoEntryLogging.png)

Code name: 'headerUncaughtException':

![[Material for MkDocs]](../../pictures/header_files/headerUncaughtException.png)

Code name: 'headerNoExitLogging':

![[Material for MkDocs]](../../pictures/header_files/headerNoExitLogging.png)

Code name: 'headerProxyCatchAll':

![[Material for MkDocs]](../../pictures/header_files/headerProxyCatchAll.png)

Code name: 'headerGenericA5':

![[Material for MkDocs]](../../pictures/header_files/headerGenericA5.png)

Code name: 'headerGenericB1':

![[Material for MkDocs]](../../pictures/header_files/headerGenericB1.png)

Code name: 'headerGenericB9':

![[Material for MkDocs]](../../pictures/header_files/headerGenericB9.png)

Code name: 'headerGenericA10':

![[Material for MkDocs]](../../pictures/header_files/headerGenericA10.png)

Code name: 'headerGenericA6':

![[Material for MkDocs]](../../pictures/header_files/headerGenericA6.png)

Code name: 'headerGenericB2':

![[Material for MkDocs]](../../pictures/header_files/headerGenericB2.png)

Code name: 'headerGenericA11':

![[Material for MkDocs]](../../pictures/header_files/headerGenericA11.png)

Code name: 'headerGenericA7':

![[Material for MkDocs]](../../pictures/header_files/headerGenericA7.png)

Code name: 'headerGenericB3':

![[Material for MkDocs]](../../pictures/header_files/headerGenericB3.png)

Code name: 'headerGenericA12':

![[Material for MkDocs]](../../pictures/header_files/headerGenericA12.png)

Code name: 'headerGenericA8':

![[Material for MkDocs]](../../pictures/header_files/headerGenericA8.png)

Code name: 'headerGenericB4':

![[Material for MkDocs]](../../pictures/header_files/headerGenericB4.png)

Code name: 'headerGenericA1':

![[Material for MkDocs]](../../pictures/header_files/headerGenericA1.png)

Code name: 'headerGenericA9':

![[Material for MkDocs]](../../pictures/header_files/headerGenericA9.png)

Code name: 'headerGenericB5':

![[Material for MkDocs]](../../pictures/header_files/headerGenericB5.png)

Code name: 'headerGenericA2':

![[Material for MkDocs]](../../pictures/header_files/headerGenericA2.png)

Code name: 'headerGenericB10':

![[Material for MkDocs]](../../pictures/header_files/headerGenericB10.png)  

Code name: 'headerGenericB6':

![[Material for MkDocs]](../../pictures/header_files/headerGenericB6.png)

Code name: 'headerGenericA3':

![[Material for MkDocs]](../../pictures/header_files/headerGenericA3.png)

Code name: 'headerGenericB11':

![[Material for MkDocs]](../../pictures/header_files/headerGenericB11.png)  

Code name: 'headerGenericB7':

![[Material for MkDocs]](../../pictures/header_files/headerGenericB7.png)

Code name: 'headerGenericA4':

![[Material for MkDocs]](../../pictures/header_files/headerGenericA4.png)

Code name: 'headerGenericB12':

![[Material for MkDocs]](../../pictures/header_files/headerGenericB12.png)

Code name: 'headerGenericB8':

![[Material for MkDocs]](../../pictures/header_files/headerGenericB8.png)


---

# Overview

# Troubleshooting

* The demo version won’t start.

```The demo version tries to download a embedded Mongo Database. You need to have an active internet connection. You can also check the log files for errors.```

* While starting the demo version an error similar to this occurred

```ERROR d.f.e.process.runtime.Starter prepare : prepare executable
java.io.IOException: Could not open inputStream for https://fastdl.mongodb.org/osx/mongodb-osx-x86_64-3.5.5.tgz
```
Solution: The application was not able to download the mongodb files. You can download those manually. Please see [Installation](../installation/installing.md)

* Where are the log files?

```The default log file directory, is named ’log’ and the folder should be next to your twx-Analyzer Jar-file.```



---

# All

## Logging Convention
Usually projects start out with the BPM internal logging mechanism. But as your project grows, you might think about advanced logging mechanisms. There is for example java.util.logging which comes with IBM BPM. You can download the sample [logging file here](./logging.js.txt). The advantage of this approach is, that your log levels are controllable through the ISC Console. And you can create a more fine grained logging, which does not clutter your log file and reduces the performance impact on your system. There are other ways to do logging, you could extract your applications logging into different files. For more information please contact a representative of the twx Solutions GmbH.

### How it works (java.util.logging)
Every service needs two additional private variables although in BAW 19 it is only one. The first variable 'serviceName' contains the service name and is set at the start script. Then it needs a loggername, for that a variable called localLoggerName needs to be initialized with a logger name that is suitable for your project. These two variables are then used by the log command. Which would look similar to this:

``` javascript hl_lines="3 4"
tw.local.serviceName = "GeneralReusableService";
tw.local.localLoggerName = "service.global";
var logMessage = "This is my message to you..."
logger.info(logMessage, tw.local.serviceName, tw.local.localLoggerName);
```

<!--
The result in the log file would be:
`TODO`
Compared to the log.info output:
`TODO`
-->

### Start- and End-Logging 
It has been proven helpful to start every service with a script that creates an info-level log entry that the service just started. The same counts for an output logging. Before every end event item, a script that creates an end logging should be introduced. This helps to a) see which service chain was called and b) a rough estimation on how long it was executed.

#### Debug 
Additionally to the info logging at the beginning and end, you should insert a debug logging, that prints all input variables at the beginning and all output variables at the end.  

Here is how it should look like: 
``` javascript hl_lines="7"
tw.local.serviceName = "GeneralReusableService";
tw.local.localLoggerName = "service.global";
if(logger.isDebug(tw.local.localLoggerName)){
    var logMessage = "InputWerte: ";
    logMessage += "analysisId = " + tw.local.analysisId;
    logMessage += ", anotherVariable = " + tw.local.anotherVariable;
    logger.debug(logMessage, tw.local.localLoggerName, tw.local.serviceName);
}
```

A tool, that generates these input and output scripts is planed for release 2.0.2

### Changes with BAW 19

With BAW 19 the variable tw.local.serviceName is no longer needed. BPM now can call its own service name

### Old log.(info|debug)

With this convention the old logging mechanism is deprecated and there will be a warning if it is used.

### Plugins with this best practice
The following plugins belong to this best practice:

* [No Entry Logging](../plugins/details/badstyle-plugins.md#no-entry-logging)

* [No Entry Logging Finer](../plugins/details/badstyle-plugins.md#no-entry-logging-finer)

* [No Entry Logging Debug](../plugins/details/badstyle-plugins.md#no-entry-logging-debug)

* [Usage Of "log.info"](../plugins/details/warning-plugins.md#usage-of-loginfo")

## UCA good practices
UCAs can be tricky, especially if they are heavily used. It is easy to loose focus on which service triggers which UCA. And in production it is even important to be able to understand what has happend. These good practices shall help improving such situations.

### UCA Missing logging

UCAs are tricky to follow and hard to analyze. Therefore it is recommended to create a logging statement whenever a UCA is either received or send. For logging if one is sent, you should create a service inside the UCA instead of using a variable and have the service create a log statement. See below picture: ![figure 3](../pictures/uca_attached_service.png "Figure 3: Uca Attached Service"). And at the receiving end you should log the message in the condition section. Don't forget to add your condition or to add a true statement at the end like in figure 4. This final 'true;' statement is crucial in order to make this work. The corresponding plugin to this best practice is called "UCA Condition Without 'return true'". This is an example:
![figure_4](../pictures/uca_attached_service_receive.png "Figure 4: Uca Logging at a receiver")

Condition example:
``` javascript
logger.finer("UCA logging example: " + tw.message.ucaVariable + " empfangen.","service.global",tw.system.process.name);
true;
```

### Plugins with this best practice
The following plugins belong to this best practice:

* [UCA Condition Without 'return true'](../plugins/details/error-plugins.md#UCA Condition Without 'return true')

## Gateways
Gateways are great if you want to make your service flow readable and not hide all your decisions in script code. But a few things are necessary to keep in mind. Gateways should always be documented and the name should be a question. Often gateways have multiple outflows and because they are poorly named developers confused the flow and insert the wrong conditions into the flow. That is why every outgoing flow of a gateway should be named as if it would answer the question.

### Plugins with this best practice
* [Switch Condition Not Implemented](../plugins/details/error-plugins.md#switch-condition-not-implemented)

## Exception Event Naming
Exceptions are not evil! At some point you have to deal with exceptions and it turned out that exceptions should be named like the service of which they originate from. The twx-Analyzer has a built-in tool, that allows you to create the exception names. It is located under section develop -> service naming. You will find detailed description about this tooling under [ServiceNaming tool](../components/developer.md)

### Plugins with this best practice
* [Exception End Event Name](../plugins/details/warning-plugins.md#exception-end-event-name)

## Naming Good Practices
Consistend naming is important for many different reasons. Those are reducint the effort for reading and understanding the code, finding elements faster and during reviews arguing about more important issues then naming syntaxes saves time and effort.

### Plugins with this best practice
* [Step Name Differs From Service Name](../plugins/details/warning-plugins.md#step-name-differs-from-service-name)
* [Element With Default Name](../plugins/details/badstyle-plugins.md#element-with-default-name)
* [Switch Output Not Named](../plugins/details/badstyle-plugins.md#switch-output-not-named)

<!-- 
## Service naming
Name services where they are used with servicename at the beginning.
-->



---

