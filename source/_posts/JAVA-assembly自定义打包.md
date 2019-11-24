---
title: '[JAVA]assembly自定义打包'
date: 2019-11-24 09:58:29
categories: JAVA
tags: maven
---

在很多场景下，会有很多shell脚本或者需要操作一些配置，可以使用maven的assembly插件打出结构清晰的架构。

官网参考：[点击进入](http://maven.apache.org/plugins/maven-assembly-plugin/assembly.html)
## 一、去掉springboot打包方式
```
<build>
    <plugins>
        <plugin>
             <groupId>org.springframework.boot</groupId>
             <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

---
## 二、添加assembly依赖
```
    <build>
        <finalName>zx-demo</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <configuration>
                    <archive>
                        <manifest>
                            <!--运行jar包时运行的主类，要求类全名-->
                            <mainClass>com.example.zxdemo.Application</mainClass>
                            <!-- 是否指定项目classpath下的依赖 -->
                            <addClasspath>true</addClasspath>
                            <!-- 指定依赖的时候声明前缀 -->
                            <classpathPrefix>./</classpathPrefix>
                        </manifest>
                        <manifestEntries>
                            <Class-Path>../conf/ ../resources/</Class-Path>
                        </manifestEntries>
                    </archive>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <descriptors>
                        <descriptor>src/main/assembly/assembly.xml</descriptor>
                    </descriptors>
                </configuration>
                <executions>
                    <execution><!-- 配置执行器 -->
                        <id>make-assembly</id>
                        <phase>package</phase><!-- 绑定到package生命周期阶段上 -->
                        <goals>
                            <goal>single</goal><!-- 只运行一次 -->
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

- 可以看到依赖中写了src/main/assembly/assembly.xml，这是配置文件存放的位置。
## 三、assembly.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<assembly>
    <id>zx-demo</id>
    <formats>
        <format>tar</format><!--打包的文件格式,也可以有：war zip-->
    </formats>
    <!--tar.gz压缩包下是否生成和项目名相同的根目录-->
    <includeBaseDirectory>false</includeBaseDirectory>
    <dependencySets>
        <dependencySet>
            <!--是否把本项目添加到依赖文件夹下-->
            <useProjectArtifact>true</useProjectArtifact>
            <outputDirectory>lib</outputDirectory>
            <!--将scope为runtime的依赖包打包-->
            <scope>runtime</scope>
        </dependencySet>
    </dependencySets>
    <fileSets>
        <fileSet>
            <directory>src/main/bin</directory>
            <outputDirectory>./</outputDirectory>
        </fileSet>
        <fileSet>
            <directory>src/main/resources</directory>
            <outputDirectory>conf</outputDirectory>
        </fileSet>
    </fileSets>
</assembly>
```

- 可以看到 src/main/bin 目录，可以自定义启动脚本。
## 四、自定义脚本
### start.sh
```
nohup java -jar -Dfile.encoding=UTF-8 -Djava.security.egd=file:/dev/urandom ./lib/zx-demo.jar > /dev/null 2>&1 &
```
### stop.sh
```
PID=`ps -ef | grep zx-demo.jar | grep -v grep | awk '{print $2}'`
if [ -z $PID ]; then
	echo Application is already stopped
else
	echo kill $PID
	kill -s 9 $PID
fi
```

在assembly.xml中可自定义打包格式，打包后，target下会生成相应的压缩文件。
![图片](http://)

conf：resources下的文件

lib：所有依赖的jar包

## 目录结构
### 项目结构
![项目](http://image.xiaoxinyes.club/20191124094523.png)

### 解压后结构
![目录](http://image.xiaoxinyes.club/20191124094451.png)
