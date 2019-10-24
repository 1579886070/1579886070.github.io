---
title: '[JAVA]Logback日志级别'
date: 2019-10-24 22:43:43
categories: JAVA
tags: java
---

<pre class="lang:default decode:true">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;!--
scan:
当此属性设置为true时，配置文件如果发生改变，将会被重新加载，默认值为true。
scanPeriod:
设置监测配置文件是否有修改的时间间隔，如果没有给出时间单位，默认单位是毫秒。当scan为true时，此属性生效。默认的时间间隔为1分钟。
debug:
当此属性设置为true时，将打印出logback内部日志信息，实时查看logback运行状态。默认值为false。
例如：
 --&gt;
&lt;configuration scan="true" scanPeriod="60 seconds" debug="true"&gt;
    &lt;!-- 项目名称 --&gt;
    &lt;property name="PROJECT_NAME" value="XXXXX" /&gt;

    &lt;!-- 文件输出格式 --&gt;
    &lt;property name="PATTERN" value="%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n"/&gt;
    &lt;!-- 输出文件路径 --&gt;
    &lt;property name="OPEN_FILE_PATH" value="./logs/"/&gt;

    &lt;appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender"&gt;
        &lt;encoder&gt;
            &lt;pattern&gt;${PATTERN}&lt;/pattern&gt;
            &lt;charset&gt;UTF-8&lt;/charset&gt;
        &lt;/encoder&gt;
    &lt;/appender&gt;

    &lt;!-- ch.qos.logback.core.rolling.RollingFileAppender 文件日志输出 --&gt;
    &lt;appender name="OPEN-FILE" class="ch.qos.logback.core.rolling.RollingFileAppender"&gt;
        &lt;!--不能有这项配置！！！！！--&gt;
        &lt;!--&lt;Encoding&gt;UTF-8&lt;/Encoding&gt;--&gt;
        &lt;!--&lt;File&gt;${OPEN_FILE_PATH}/zqread.log&lt;/File&gt;--&gt;
        &lt;rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy"&gt;
            &lt;!--日志文件输出的文件名--&gt;
            &lt;FileNamePattern&gt;${OPEN_FILE_PATH}/all/%d{yyyy-MM-dd}-%i.log&lt;/FileNamePattern&gt;
            &lt;!--日志文件保留天数--&gt;
            &lt;MaxHistory&gt;30&lt;/MaxHistory&gt;
            &lt;TimeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP"&gt;
                &lt;!--日志文件最大的大小--&gt;
                &lt;MaxFileSize&gt;10MB&lt;/MaxFileSize&gt;
            &lt;/TimeBasedFileNamingAndTriggeringPolicy&gt;
        &lt;/rollingPolicy&gt;

        &lt;layout class="ch.qos.logback.classic.PatternLayout"&gt;
            &lt;pattern&gt;${PATTERN}&lt;/pattern&gt;
        &lt;/layout&gt;
    &lt;/appender&gt;

    &lt;!--输出到debug--&gt;
    &lt;appender name="debug" class="ch.qos.logback.core.rolling.RollingFileAppender"&gt;
        &lt;rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy"&gt;
            &lt;FileNamePattern&gt;${OPEN_FILE_PATH}/debug/%d{yyyy-MM-dd}-%i.log&lt;/FileNamePattern&gt;
            &lt;MaxHistory&gt;30&lt;/MaxHistory&gt;
            &lt;TimeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP"&gt;
                &lt;MaxFileSize&gt;10MB&lt;/MaxFileSize&gt;
            &lt;/TimeBasedFileNamingAndTriggeringPolicy&gt;
        &lt;/rollingPolicy&gt;
        &lt;append&gt;true&lt;/append&gt;
        &lt;encoder&gt;
            &lt;pattern&gt;%d{yyyy-MM-dd HH:mm:ss.SSS} %contextName [%thread] %-5level %logger{36} - %msg%n&lt;/pattern&gt;
            &lt;charset&gt;utf-8&lt;/charset&gt;
        &lt;/encoder&gt;
        &lt;filter class="ch.qos.logback.classic.filter.LevelFilter"&gt;&lt;!-- 只打印DEBUG日志 --&gt;
            &lt;level&gt;DEBUG&lt;/level&gt;
            &lt;onMatch&gt;ACCEPT&lt;/onMatch&gt;
            &lt;onMismatch&gt;DENY&lt;/onMismatch&gt;
        &lt;/filter&gt;
    &lt;/appender&gt;

    &lt;!--输出到info--&gt;
    &lt;appender name="info" class="ch.qos.logback.core.rolling.RollingFileAppender"&gt;
        &lt;rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy"&gt;
            &lt;FileNamePattern&gt;${OPEN_FILE_PATH}/info/%d{yyyy-MM-dd}-%i.log&lt;/FileNamePattern&gt;
            &lt;MaxHistory&gt;30&lt;/MaxHistory&gt;
            &lt;TimeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP"&gt;
                &lt;MaxFileSize&gt;10MB&lt;/MaxFileSize&gt;
            &lt;/TimeBasedFileNamingAndTriggeringPolicy&gt;
        &lt;/rollingPolicy&gt;
        &lt;append&gt;true&lt;/append&gt;
        &lt;encoder&gt;
            &lt;pattern&gt;%d{yyyy-MM-dd HH:mm:ss.SSS} %contextName [%thread] %-5level %logger{36} - %msg%n&lt;/pattern&gt;
            &lt;charset&gt;utf-8&lt;/charset&gt;
        &lt;/encoder&gt;
        &lt;filter class="ch.qos.logback.classic.filter.LevelFilter"&gt;&lt;!-- 只打印INFO日志 --&gt;
            &lt;level&gt;INFO&lt;/level&gt;
            &lt;onMatch&gt;ACCEPT&lt;/onMatch&gt;
            &lt;onMismatch&gt;DENY&lt;/onMismatch&gt;
        &lt;/filter&gt;
    &lt;/appender&gt;

    &lt;!--输出到error--&gt;
    &lt;appender name="error" class="ch.qos.logback.core.rolling.RollingFileAppender"&gt;
        &lt;rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy"&gt;
            &lt;FileNamePattern&gt;${OPEN_FILE_PATH}/error/%d{yyyy-MM-dd}-%i.log&lt;/FileNamePattern&gt;
            &lt;MaxHistory&gt;30&lt;/MaxHistory&gt;
            &lt;TimeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP"&gt;
                &lt;MaxFileSize&gt;10MB&lt;/MaxFileSize&gt;
            &lt;/TimeBasedFileNamingAndTriggeringPolicy&gt;
        &lt;/rollingPolicy&gt;
        &lt;append&gt;true&lt;/append&gt;
        &lt;encoder&gt;
            &lt;pattern&gt;%d{yyyy-MM-dd HH:mm:ss.SSS} %contextName [%thread] %-5level %logger{36} - %msg%n&lt;/pattern&gt;
            &lt;charset&gt;utf-8&lt;/charset&gt;
        &lt;/encoder&gt;
        &lt;filter class="ch.qos.logback.classic.filter.LevelFilter"&gt;&lt;!-- 只打印ERROR日志 --&gt;
            &lt;level&gt;ERROR&lt;/level&gt;
            &lt;onMatch&gt;ACCEPT&lt;/onMatch&gt;
            &lt;onMismatch&gt;DENY&lt;/onMismatch&gt;
        &lt;/filter&gt;
    &lt;/appender&gt;

    &lt;!--输出到warn--&gt;
    &lt;appender name="warn" class="ch.qos.logback.core.rolling.RollingFileAppender"&gt;
        &lt;rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy"&gt;
            &lt;FileNamePattern&gt;${OPEN_FILE_PATH}/warn/%d{yyyy-MM-dd}-%i.log&lt;/FileNamePattern&gt;
            &lt;MaxHistory&gt;30&lt;/MaxHistory&gt;
            &lt;TimeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP"&gt;
                &lt;MaxFileSize&gt;10MB&lt;/MaxFileSize&gt;
            &lt;/TimeBasedFileNamingAndTriggeringPolicy&gt;
        &lt;/rollingPolicy&gt;
        &lt;append&gt;true&lt;/append&gt;
        &lt;encoder&gt;
            &lt;pattern&gt;%d{yyyy-MM-dd HH:mm:ss.SSS} %contextName [%thread] %-5level %logger{36} - %msg%n&lt;/pattern&gt;
            &lt;charset&gt;utf-8&lt;/charset&gt;
        &lt;/encoder&gt;
        &lt;filter class="ch.qos.logback.classic.filter.LevelFilter"&gt;&lt;!-- 只打印WARN日志 --&gt;
            &lt;level&gt;WARN&lt;/level&gt;
            &lt;onMatch&gt;ACCEPT&lt;/onMatch&gt;
            &lt;onMismatch&gt;DENY&lt;/onMismatch&gt;
        &lt;/filter&gt;
    &lt;/appender&gt;

    &lt;root level="info"&gt;
        &lt;appender-ref ref="STDOUT"/&gt;
        &lt;appender-ref ref="OPEN-FILE"/&gt;
        &lt;appender-ref ref="debug" /&gt;
        &lt;appender-ref ref="info" /&gt;
        &lt;appender-ref ref="error" /&gt;
        &lt;appender-ref ref="warn" /&gt;
    &lt;/root&gt;
&lt;/configuration&gt;</pre>
&nbsp;
