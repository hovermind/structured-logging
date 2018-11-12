# TOC
* [Dependencies](#)
* [Standards LoggingEvent Fields](https://github.com/logstash/logstash-logback-encoder#standard-fields)

## Dependencies
```xml
<dependency>
	<groupId>ch.qos.logback</groupId>
	<artifactId>logback-core</artifactId>
</dependency>

<dependency>
	<groupId>ch.qos.logback</groupId>
	<artifactId>logback-classic</artifactId>
</dependency>

<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-databind</artifactId>
</dependency>

<dependency>
	<groupId>net.logstash.logback</groupId>
	<artifactId>logstash-logback-encoder</artifactId>
	<version>5.2</version>
</dependency>
```
**Note:** SLF4J, logback-core, logback-classic & jackson-databind are included in spring boot

## Customizing Standard Field Names
```xml
<encoder class="net.logstash.logback.encoder.LogstashEncoder">
  <fieldNames>
    <timestamp>time</timestamp>
    <message>msg</message>
    ...
  </fieldNames>
</encoder>
```
## Ignoring fields
```xml
<includeContext>false</includeContext>
<includeCallerData>fasle</includeCallerData>
<fieldNames>
    <timestamp>timestamp</timestamp>
    <message>message</message>
    <thread>[ignore]</thread>
    <logger>[ignore]</logger>
    <version>[ignore]</version>
    <levelValue>[ignore]</levelValue>
    <caller>[ignore]</caller>
    <stackTrace>[ignore]</stackTrace>
    <mdc>context</mdc>
</fieldNames>
```

## [Customizing Version](https://github.com/logstash/logstash-logback-encoder#customizing-version)
The version field value by default is the string value 1. The value can be changed like this:
```xml
<encoder class="net.logstash.logback.encoder.LogstashEncoder">
  <version>2</version>
</encoder>
```

The value can be written as a number (instead of a string) like this:
```xml
<encoder class="net.logstash.logback.encoder.LogstashEncoder">
  <writeVersionAsInteger>true</writeVersionAsInteger>
</encoder>
```

## [Customizing Timestamp](https://github.com/logstash/logstash-logback-encoder#customizing-timestamp)
By default, timestamps are written as string values in the format yyyy-MM-dd'T'HH:mm:ss.SSSZZ (e.g. 2018-04-28T22:23:59.164-07:00), in the default TimeZone of the host Java platform.

You can change the timezone like this:
```xml
<encoder class="net.logstash.logback.encoder.LogstashEncoder">
  <timeZone>UTC</timeZone>
</encoder>
```

The value of the timeZone element can be any string accepted by java's TimeZone.getTimeZone(String id) method.

You can change the pattern used like this:
```xml
<encoder class="net.logstash.logback.encoder.LogstashEncoder">
  <timestampPattern>yyyy-MM-dd'T'HH:mm:ss.SSS</timestampPattern>
</encoder>
```

Use these timestamp pattern values to output the timestamp as a unix timestamp (number of milliseconds since unix epoch).
* [UNIX_TIMESTAMP_AS_NUMBER] - write the timestamp value as a numeric unix timestamp
* [UNIX_TIMESTAMP_AS_STRING] - write the timestamp value as a string verion of the numeric unix timestamp

For example:
```xml
<encoder class="net.logstash.logback.encoder.LogstashEncoder">
  <timestampPattern>[UNIX_TIMESTAMP_AS_NUMBER]</timestampPattern>
</encoder>
```

## [ShortenedFieldNames](https://github.com/logstash/logstash-logback-encoder/blob/master/src/main/java/net/logstash/logback/fieldnames/ShortenedFieldNames.java)
```xml
<encoder class="net.logstash.logback.encoder.LogstashEncoder">
  <fieldNames class="net.logstash.logback.fieldnames.ShortenedFieldNames"/>
</encoder>
```

## [Composite Encoder/Layout](https://github.com/logstash/logstash-logback-encoder#composite-encoderlayout)
* Composite encoders/layouts are composed of one or more JSON providers that contribute to the JSON output
* No providers are configured by default in the composite encoders/layouts. You must add the ones you want
* The logstash-logback-encoder library contains many providers out-of-the-box
* You can even plug-in your own by extending JsonProvider

**See:**
* [Nested JSON Provider](https://github.com/logstash/logstash-logback-encoder#nested-json-provider)
* [Pattern JSON Provider](https://github.com/logstash/logstash-logback-encoder#pattern-json-provider)
* [LoggingEvent patterns](https://github.com/logstash/logstash-logback-encoder#loggingevent-patterns)
* [Custom JSON Provider](https://github.com/logstash/logstash-logback-encoder#custom-json-provider)
