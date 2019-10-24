---
title: '[JAVA]Maven打包到本地仓库'
date: 2019-10-24 22:42:32
categories: JAVA
tags: java
---

有些jar中央仓库没得，那么可以自己把jar添加到本地maven仓库使用。<!--more-->
<pre class="lang:default decode:true">mvn install:install-file -DgroupId=signalr-client-sdk -DartifactId=signalr-client-sdk -Dversion=1.0.0 -Dfile=G:\signalr-client-sdk-1.0.0.jar -Dpackaging=jar</pre>
&nbsp;

-DgroupId：组织名
-DartifactId：项目名
-Dversion：版本号
-Dfile：jar包路径
-Dpackaging：jar

&nbsp;

maven依赖
<pre class="lang:default decode:true ">&lt;dependency&gt;
	&lt;groupId&gt;signalr-client-sdk&lt;/groupId&gt;
	&lt;artifactId&gt;signalr-client-sdk&lt;/artifactId&gt;
	&lt;version&gt;1.0.0&lt;/version&gt;
&lt;/dependency&gt;</pre>
&nbsp;
