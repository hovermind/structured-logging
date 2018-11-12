## Logback dependency for spring boot
`pom.xml`
```xml
<dependency>
	<groupId>ch.qos.logback</groupId>
	<artifactId>logback-classic</artifactId>
</dependency>
```
**Note:**
* logback-classic is included with spring boot, that's why version is not needed
* If you are not using spring boot, you have to provide SLF4J, logback-core & logback classic dependencies with version
