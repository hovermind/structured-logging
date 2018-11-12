# TOC
* [Dependency](/logback-logging-json.md#dependency)
* [Logback configuration](/logback-logging-json.md#logback-configuration)
* [MDC - Mapped Diagnostic Context](/logback-logging-json.md#mdc---mapped-diagnostic-context)
* [Custom JSON layout](/logback-logging-json.md#custom-json-layout)
* [MDCInsertingServletFilter](/logback-logging-json.md#mdcinsertingservletfilter)

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

## [MDC - Mapped Diagnostic Context](https://logback.qos.ch/manual/mdc.html)
MDC inserts key-value pair before log message
```java
MDC.put("foo-key", "foo-value");
fooLogger.info("Foo info");
MDC.clear();
```

Log output
```json
{
  "timestamp" : "...",
  "level" : "...",
  "mdc" : {
    "foo-key" : "foo-value"
  },
  "logger" : "...",
  "message" : "Foo info"
}
```

#### Accessing MDC value directly
`%X{foo-key}`
```xml
<appender name="consoleAppenderJson" class="ch.qos.logback.core.ConsoleAppender">
	<encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
		<Pattern>
			%-5level -- FOO: "%X{foo-key}", MESSAGE: %message%n
		</Pattern>
	</encoder>       
</appender>  
```

Log output
```
INFO -- FOO: "foo-value", MESSAGE: "foo message"
```


**Note:** `PatternLayoutEncoder` is sub-class of `LayoutWrappingEncoder` and only allows `<pattern></pattern>` (`<layout></layout>` is not allowed)


## Custom JSON layout
#### 1. Extending JsonLayout
```java
public class FooJsonLayout extends JsonLayout {
	
	@Override
	protected void addCustomDataToJsonMap(Map<String, Object> map, ILoggingEvent event){
		map.put("app-name", "FooApp");
	}
}
```

Log output
```json
{
  "timestamp" : "...",
  "level" : "...",
  "mdc" : {
    "app-name" : "FooApp"
  },
  "logger" : "...",
  "message" : "..."
}
```

#### 2. Using PatternLayoutEncoder
```xml
<appender name="consoleAppenderCustomJson" class="ch.qos.logback.core.ConsoleAppender">
	<encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
		<pattern>
			{ "timestamp" : "%d{yyyy-MM-dd HH:mm:ss}", "level" : "%level", "message" : "%message", "foo": "%X{foo-key}" }%n
		</pattern>
	</encoder>
</appender>


<root level="error">
	<appender-ref ref="consoleAppenderCustomJson" />
</root>

<logger name="fooLogger" level="debug" additivity="false">
	<appender-ref ref="consoleAppenderCustomJson" />
</logger>
```

`FooClass.java`
```java
protected final Logger fooLogger = LoggerFactory.getLogger("fooLogger");

MDC.put("foo-key", "foo-value");
fooLogger.info("foo-message");
MDC.clear();
```

Log output
```json
{ "timestamp" : "2018-11-08 17:21:06", "level" : "INFO", "message" : "foo-message", "foo": "foo-value"} }
```

**Note:** `PatternLayoutEncoder` is default, so you can use `<encoder>` without `class="ch.qos.logback.classic.encoder.PatternLayoutEncoder"`:

## MDCInsertingServletFilter
MDCInsertingServletFilter inserts hostname, request uri and user-agent etc. (associated with a given HTTP request) into the MDC

#### Step - 1 : put at top of `web.xml`
```xml
<filter>
  <filter-name>MDCInsertingServletFilter</filter-name>
  <filter-class>
    ch.qos.logback.classic.helpers.MDCInsertingServletFilter
  </filter-class>
</filter>
<filter-mapping>
  <filter-name>MDCInsertingServletFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping> 
```

#### Step - 2 use [MDC keys](https://logback.qos.ch/manual/mdc.html#mis) in encoder
Using MDC key `req.requestURI` (`%X{req.requestURI}`)
```xml
<appender name="requestJsonConsoleAppender" class="ch.qos.logback.core.ConsoleAppender">
	<encoder>
		<pattern>[%d{yyyy-MM-dd HH:mm:ss} %-5level] %message => { &quot;Uri&quot;: &quot;%X{req.requestURI}&quot;, RequestBody: %X{RequestBody} }%n</pattern>
	</encoder>
</appender>
```





