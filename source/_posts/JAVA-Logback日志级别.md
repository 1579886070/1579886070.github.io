---
title: '[JAVA]Logback日志级别'
date: 2019-10-24 22:43:43
categories: JAVA
tags: java
---

```
<?xml version="1.0" encoding="UTF-8"?>
<!--
scan:
当此属性设置为true时，配置文件如果发生改变，将会被重新加载，默认值为true。
scanPeriod:
设置监测配置文件是否有修改的时间间隔，如果没有给出时间单位，默认单位是毫秒。当scan为true时，此属性生效。默认的时间间隔为1分钟。
debug:
当此属性设置为true时，将打印出logback内部日志信息，实时查看logback运行状态。默认值为false。
例如：
 -->
<configuration scan="true" scanPeriod="60 seconds" debug="true">
    <!-- 项目名称 -->
    <property name="PROJECT_NAME" value="XXXXX" />

    <!-- 文件输出格式 -->
    <property name="PATTERN" value="%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n"/>
    <!-- 输出文件路径 -->
    <property name="OPEN_FILE_PATH" value="./logs/"/>

    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>${PATTERN}</pattern>
            <charset>UTF-8</charset>
        </encoder>
    </appender>

    <!-- ch.qos.logback.core.rolling.RollingFileAppender 文件日志输出 -->
    <appender name="OPEN-FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!--不能有这项配置！！！！！-->
        <!--<Encoding>UTF-8</Encoding>-->
        <!--<File>${OPEN_FILE_PATH}/zqread.log</File>-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志文件输出的文件名-->
            <FileNamePattern>${OPEN_FILE_PATH}/all/%d{yyyy-MM-dd}-%i.log</FileNamePattern>
            <!--日志文件保留天数-->
            <MaxHistory>30</MaxHistory>
            <TimeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <!--日志文件最大的大小-->
                <MaxFileSize>10MB</MaxFileSize>
            </TimeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>

        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>${PATTERN}</pattern>
        </layout>
    </appender>

    <!--输出到debug-->
    <appender name="debug" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <FileNamePattern>${OPEN_FILE_PATH}/debug/%d{yyyy-MM-dd}-%i.log</FileNamePattern>
            <MaxHistory>30</MaxHistory>
            <TimeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <MaxFileSize>10MB</MaxFileSize>
            </TimeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
        <append>true</append>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} %contextName [%thread] %-5level %logger{36} - %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter"><!-- 只打印DEBUG日志 -->
            <level>DEBUG</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!--输出到info-->
    <appender name="info" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <FileNamePattern>${OPEN_FILE_PATH}/info/%d{yyyy-MM-dd}-%i.log</FileNamePattern>
            <MaxHistory>30</MaxHistory>
            <TimeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <MaxFileSize>10MB</MaxFileSize>
            </TimeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
        <append>true</append>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} %contextName [%thread] %-5level %logger{36} - %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter"><!-- 只打印INFO日志 -->
            <level>INFO</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!--输出到error-->
    <appender name="error" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <FileNamePattern>${OPEN_FILE_PATH}/error/%d{yyyy-MM-dd}-%i.log</FileNamePattern>
            <MaxHistory>30</MaxHistory>
            <TimeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <MaxFileSize>10MB</MaxFileSize>
            </TimeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
        <append>true</append>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} %contextName [%thread] %-5level %logger{36} - %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter"><!-- 只打印ERROR日志 -->
            <level>ERROR</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!--输出到warn-->
    <appender name="warn" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <FileNamePattern>${OPEN_FILE_PATH}/warn/%d{yyyy-MM-dd}-%i.log</FileNamePattern>
            <MaxHistory>30</MaxHistory>
            <TimeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <MaxFileSize>10MB</MaxFileSize>
            </TimeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
        <append>true</append>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} %contextName [%thread] %-5level %logger{36} - %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter"><!-- 只打印WARN日志 -->
            <level>WARN</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <root level="info">
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="OPEN-FILE"/>
        <appender-ref ref="debug" />
        <appender-ref ref="info" />
        <appender-ref ref="error" />
        <appender-ref ref="warn" />
    </root>
</configuration>
```
