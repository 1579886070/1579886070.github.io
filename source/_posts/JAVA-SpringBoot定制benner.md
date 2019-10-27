---
title: '[JAVA]SpringBoot定制benner'
date: 2019-10-27 19:56:08
categories: JAVA
tags: java
---

springboot默认启动banner如下：

![默认banner](
http://image.xiaoxinyes.club/2019-10-27_195347.png)

修改步骤也十分简单

---
## 修改步骤
### 一
在src/main/resources下新建名为banner.txt的文件。

### 二
文本中写入需要的字符，可通过[点击进入](http://patorjk.com/software/taag)该网站生成字符。
![生成字符网站](
http://image.xiaoxinyes.club/2019-10-27_200333.png)

### 三
启动程序即可，可以看到已经发生了变化
![修改后的benner](
http://image.xiaoxinyes.club/2019-10-27_200550.png)

## 关闭benner
```
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication app = new SpringApplication(DemoApplication.class);
        app.setBannerMode(Banner.Mode.OFF);
        app.run(args);  
    }

}
```
