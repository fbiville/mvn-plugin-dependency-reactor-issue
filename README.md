# Maven Reactor issue with Plugin Dependencies

```
.
├── README.md
├── a-module (JAR)
│   └── pom.xml
├── plugin-dependency (JAR)
│   └── pom.xml
└── pom.xml
```

The checkstyle plugin is configured with an extra dependency: `plugin-dependency`.

Let's run checkstyle (default goal) against `a-module`:

```shell
mvn -am -pl a-module
```
Oh, no!
```shell
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-checkstyle-plugin:3.1.2:check (default-cli)
 on project mvn-plugin-dependency-reactor-issue: 
 Execution default-cli of goal org.apache.maven.plugins:maven-checkstyle-plugin:3.1.2:check failed: 
 Plugin org.apache.maven.plugins:maven-checkstyle-plugin:3.1.2 or one of its dependencies could not be resolved: 
 Could not find artifact com.acme:plugin-dependency:jar:1.0-SNAPSHOT in apache.snapshots (https://repository.apache.org/snapshots) -> [Help 1]
```

Maybe let's give a hint to Maven Reactor and explicitly list the dependency:
```shell
mvn -am -pl plugin-dependency,a-module
```
Same error 😭

This is reproduced with Maven `3.8.1` and `4.0.0-alpha-1-SNAPSHOT` (`1e95011a3088caeb6b0ad7a62becef542ab453a7`).