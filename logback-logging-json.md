# TOC
* [dependency](#)

## Dependency
`pom.xml`
```xml
<dependency>
	<groupId>ch.qos.logback.contrib</groupId>
	<artifactId>logback-json-classic</artifactId>
	<version>0.1.5</version>
</dependency>

<dependency>
	<groupId>ch.qos.logback.contrib</groupId>
	<artifactId>logback-jackson</artifactId>
	<version>0.1.5</version>
</dependency>

<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-databind</artifactId>
</dependency>
```

**Note:** `jackson-databind` is included with spring boot, that's why version is not needed

## Logback configuration
`logback.xml` (for spring boot: `logback-spring.xml`)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true" scan="true">

	<include resource="org/springframework/boot/logging/logback/defaults.xml" />

	<springProfile name="dev">

		<appender name="consoleAppenderJson" class="ch.qos.logback.core.ConsoleAppender">

			<encoder class="ch.qos.logback.core.encoder.LayoutWrappingEncoder">

				<layout class="ch.qos.logback.contrib.json.classic.JsonLayout">
				
					<timestampFormat>yyy-MM-dd' 'HH:mm:ss</timestampFormat>
					
					<jsonFormatter
						class="ch.qos.logback.contrib.jackson.JacksonJsonFormatter">
						<prettyPrint>true</prettyPrint>
					</jsonFormatter>
					
				</layout>

			</encoder>

		</appender>


		<root level="error">
			<appender-ref ref="consoleAppenderJson" />
		</root>

		<logger name="fooPackage" level="debug" additivity="false">
			<appender-ref ref="consoleAppenderJson" />
		</logger>

	</springProfile>
	
</configuration>
```
