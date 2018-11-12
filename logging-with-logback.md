## Using logback for sping boot app
* crete `logback-spring.xml` in resources folder of spring boot app
* target a spring profile (i.e. `papplication-dev.xml` => `<springProfile name="dev">`)

**Note:** spring boot includes all dependency for logback (i.e. SLF4J, logback-core, logback-classic, jackson-databind etc). If you are not using spring boot you have specify these dependencies in `pom.xml`

## Logback configuration for spring boot
`logback-spring.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true" scan="true">

	<include resource="org/springframework/boot/logging/logback/defaults.xml" />

	<variable name="FILE_LOG_PATTERN"
		value="%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{0}.%method[%line] --- %message%n" />
		
	<property name="LOG_FILE" value="C:/logs/mc.log" />
	
	<springProfile name="dev">

		<appender name="fooConsoleAppender" class="ch.qos.logback.core.ConsoleAppender">
			<encoder>
				<pattern>${FILE_LOG_PATTERN}</pattern>
			</encoder>
		</appender>

		<appender name="fooRollingFileAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">
			<file>${LOG_FILE}</file>
			<encoder>
				<pattern>${FILE_LOG_PATTERN}</pattern>
			</encoder>
			<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
				<fileNamePattern>${LOG_FILE}.%d{yyyy-MM-dd}.log</fileNamePattern>
			</rollingPolicy>
		</appender>

		<root level="ERROR">
			<appender-ref ref="fooConsoleAppender" />
			<appender-ref ref="fooRollingFileAppender />
		</root>

		<logger name="fooPackage" level="DEBUG" additivity="false">
			<appender-ref ref="fooConsoleAppender" />
			<appender-ref ref="fooRollingFileAppender />
		</logger>

		<logger name="org.springframework" level="ERROR">
			<appender-ref ref="fooConsoleAppender" />
			<appender-ref ref="fooRollingFileAppender />
		</logger>

		<logger name="org.mongodb" level="ERROR">
			<appender-ref ref="fooConsoleAppender" />
			<appender-ref ref="fooRollingFileAppender />
		</logger>

	</springProfile>

	<springProfile name="production">


	</springProfile>

</configuration>
```
