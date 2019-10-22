---
title: '[JAVA]文件上传'
date: 2019-10-22 22:40:28
categories: JAVA
tags: java
---

关于文件上传有几个需要注意的地方！<!--more-->

*   静态资源需要放到static文件下。
*   form表单提交属性为enctype="multipart/form-data"。
*   设置上传文件的默认大小值。
&nbsp;

## 1.index.html

static文件下建立index.htnl页面
<pre class="lang:default decode:true" title="index.html">&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
    &lt;meta charset="UTF-8"&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;form action="upload" method="post" enctype="multipart/form-data"&gt;
    &lt;input type="file" name="fileupload"&gt;&lt;br/&gt;
    &lt;input type="submit" value="提交"&gt;
&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>
&nbsp;

## 2.编写Contorller

<pre class="lang:java decode:true" title="FileUploadController.java">package xyz.xioaxin12.springboot.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

import java.io.File;
import java.io.IOException;

/**
 * @author 爱生活爱技术
 */
@RestController
public class FileUploadController {
    @RequestMapping("/upload")
    public String fileUpload(@RequestParam("fileupload") MultipartFile file) throws IOException {
        System.out.println(file.getOriginalFilename());
        //保存到本地g盘
        file.transferTo(new File("g://"+file.getOriginalFilename()));
        return "OK！";
    }
}
</pre>
&nbsp;

## 3.启动类

<pre class="lang:java decode:true">package xyz.xioaxin12.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringBootUploadApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootUploadApplication.class, args);
    }

}

</pre>
&nbsp;

关于文件大小

当上传的文件超出时，需要自己设置最大值。

application.properties设置如下配置
<pre class="lang:default decode:true">#设置单个上传文件的大小
spring.servlet.multipart.max-file-size=100MB
#设置一次请求上传文件的总容量
spring.servlet.multipart.max-request-size=100MB</pre>
&nbsp;
