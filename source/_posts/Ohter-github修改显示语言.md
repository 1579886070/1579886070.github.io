---
title: '[Ohter]github修改显示语言'
date: 2019-10-11 20:24:16
categories: Ohter
tags: github
---

将一个java项目git到GitHub上，语言显示的css。也许是css文件比较多吧，自动识别成了css的了。

&nbsp;

### 修改方法

在目录创建一个<span style="color: #ff0000;">.gitattributes</span>文件。

![](http://image.xiaoxinyes.club/2019-01-04_134342.png)

&nbsp;

创建之后，添加需要指定的语言即可。

![](http://image.xiaoxinyes.club/2019-01-04_134514.png)

&nbsp;
<pre class="lang:default decode:true" title=".gitattributes">*.js linguist-language=java
*.css linguist-language=java
*.html linguist-language=java</pre>
&nbsp;
