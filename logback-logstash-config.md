`logback-spring.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true" scan="true">

	<include resource="org/springframework/boot/logging/logback/defaults.xml" />

	<variable name="LOG_FILE_LOCATION_WINDOWS" value="C:/logs/foo.log" />

	<variable name="LOG_FILE_LOCATION_LINUX" value="${catalina.home}/logs/foo.log" />

	<variable name="DEFAULT_LOG_PATTERN"
		value="%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{0}.%method[%line] --- %message%n" />

	<variable name="REQUEST_JSON_LOG_PATTERN"
		value="[%d{yyyy-MM-dd HH:mm:ss} %-5level] %message => { &quot;Uri&quot;: &quot;%X{req.requestURI}&quot;, RequestBody: %X{RequestBody} }%n" />

	<variable name="SUCCESSFUL_RESPONSE_JSON_LOG_PATTERN"
		value="[%d{yyyy-MM-dd HH:mm:ss} %-5level] %message => { &quot;Code&quot;: &quot;%X{Code}&quot;, ResponseBody: %X{ResponseBody} }%n" />

	<variable name="FAILED_RESPONSE_LOG_PATTERN"
		value="[%d{yyyy-MM-dd HH:mm:ss} %-5level] %message => { &quot;Code&quot;: &quot;%X{Code}&quot;, ErrorBody: %X{ErrorBody} }%n" />

	<springProfile name="dev">

		<appender name="defaultConsoleAppender" class="ch.qos.logback.core.ConsoleAppender">
			<encoder class="net.logstash.logback.encoder.LogstashEncoder">
				<customFields>{ "appname" : "foo" }</customFields>
			</encoder>
		</appender>

		<appender name="requestJsonConsoleAppender" class="ch.qos.logback.core.ConsoleAppender">
			<encoder class="net.logstash.logback.encoder.LogstashEncoder">
				<customFields>{ "appname" : "foo" }</customFields>
			</encoder>
		</appender>

		<appender name="responseJsonConsoleAppender" class="ch.qos.logback.core.ConsoleAppender">
			<encoder class="net.logstash.logback.encoder.LogstashEncoder">
				<customFields>{ "appname" : "foo" }</customFields>
			</encoder>
		</appender>

		<appender name="failedResponseConsoleAppender" class="ch.qos.logback.core.ConsoleAppender">
			<encoder class="net.logstash.logback.encoder.LogstashEncoder">
				<customFields>{ "appname" : "foo" }</customFields>
			</encoder>
		</appender>

		<appender name="logstashFileAppender"
			class="ch.qos.logback.core.rolling.RollingFileAppender">
			<filter class="ch.qos.logback.classic.filter.ThresholdFilter">
				<level>debug</level>
			</filter>
			<file>${LOG_FILE_LOCATION_WINDOWS}</file>
			<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
				<fileNamePattern>${LOG_FILE}.%d{yyyy-MM-dd}.log</fileNamePattern>
				<maxHistory>30</maxHistory>
			</rollingPolicy>
			<encoder class="net.logstash.logback.encoder.LogstashEncoder" />
		</appender>


		<root level="error">
			<appender-ref ref="defaultConsoleAppender" />
		</root>

		<logger name="ccps" level="debug" additivity="false">
			<appender-ref ref="defaultConsoleAppender" />
			<appender-ref ref="logstashFileAppender" />
		</logger>

		<logger name="requestLogger" level="debug" additivity="false">
			<appender-ref ref="requestJsonConsoleAppender" />
			<appender-ref ref="logstashFileAppender" />
		</logger>

		<logger name="successResponseLogger" level="debug" additivity="false">
			<appender-ref ref="responseJsonConsoleAppender" />
			<appender-ref ref="logstashFileAppender" />
		</logger>

		<logger name="failResponseLogger" level="error" additivity="false">
			<appender-ref ref="failedResponseConsoleAppender" />
			<appender-ref ref="logstashFileAppender" />
		</logger>


	</springProfile>

	<springProfile name="production">


	</springProfile>

</configuration>
```
