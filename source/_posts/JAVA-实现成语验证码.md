---
title: '[JAVA]实现成语验证码'
date: 2019-10-16 18:36:58
categories: JAVA
tags: java
---

## 生成验证码，WEB-INF下一个idiom.txt文件，存放四字成语。

<pre class="lang:java decode:true ">package xyz.xioaxin12.utils;

import ...

public class ImgToolServlet extends HttpServlet {

    // 集合中保存所有成语
    private List&lt;String&gt; words = new ArrayList&lt;String&gt;();

    @Override
    public void init() throws ServletException {
        // 初始化阶段，读取new_words.txt
        // web工程中读取 文件，必须使用绝对磁盘路径
        String path = getServletContext().getRealPath("/WEB-INF/idiom.txt");
        try {
            BufferedReader reader = new BufferedReader(new InputStreamReader(
                    new FileInputStream(path), "utf-8"));
            String line;
            while ((line = reader.readLine()) != null) {
                words.add(line);
            }
            reader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 禁止缓存
        // response.setHeader("Cache-Control", "no-cache");
        // response.setHeader("Pragma", "no-cache");
        // response.setDateHeader("Expires", -1);

        int width = 120;
        int height = 30;

        // 步骤一 绘制一张内存中图片
        BufferedImage bufferedImage = new BufferedImage(width, height,
                BufferedImage.TYPE_INT_RGB);

        // 步骤二 图片绘制背景颜色 ---通过绘图对象
        Graphics graphics = bufferedImage.getGraphics();// 得到画图对象 --- 画笔
        // 绘制任何图形之前 都必须指定一个颜色
        graphics.setColor(getRandColor(200, 250));
        graphics.fillRect(0, 0, width, height);

        // 步骤三 绘制边框
        graphics.setColor(Color.WHITE);
        graphics.drawRect(0, 0, width - 1, height - 1);

        // 步骤四 四个随机数字
        Graphics2D graphics2d = (Graphics2D) graphics;
        // 设置输出字体
        graphics2d.setFont(new Font("宋体", Font.BOLD, 18));

        Random random = new Random();// 生成随机数
        int index = random.nextInt(words.size());
        String word = words.get(index);// 获得成语

        // 定义x坐标
        int x = 10;
        for (int i = 0; i &lt; word.length(); i++) {
            // 随机颜色
            graphics2d.setColor(new Color(20 + random.nextInt(110), 20 + random
                    .nextInt(110), 20 + random.nextInt(110)));
            // 旋转 -30 --- 30度
            int jiaodu = random.nextInt(60) - 30;
            // 换算弧度
            double theta = jiaodu * Math.PI / 180;

            // 获得字母数字
            char c = word.charAt(i);

            // 将c 输出到图片
            graphics2d.rotate(theta, x, 20);
            graphics2d.drawString(String.valueOf(c), x, 20);
            graphics2d.rotate(-theta, x, 20);
            x += 30;
        }

        // 将验证码内容保存session
        request.getSession().setAttribute("checkcode_session", word);

        System.out.println(word);

        // 步骤五 绘制干扰线
        graphics.setColor(getRandColor(160, 200));
        int x1;
        int x2;
        int y1;
        int y2;
        for (int i = 0; i &lt; 30; i++) {
            x1 = random.nextInt(width);
            x2 = random.nextInt(12);
            y1 = random.nextInt(height);
            y2 = random.nextInt(12);
            graphics.drawLine(x1, y1, x1 + x2, x2 + y2);
        }

        // 将上面图片输出到浏览器 ImageIO
        graphics.dispose();// 释放资源
        ImageIO.write(bufferedImage, "jpg", response.getOutputStream());

    }

    @Override
    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        doGet(request, response);
    }

    /**
     * 取其某一范围的color
     *
     * @param fc int 范围参数1
     * @param bc int 范围参数2
     * @return Color
     */
    private Color getRandColor(int fc, int bc) {
        // 取其随机颜色
        Random random = new Random();
        if (fc &gt; 255) {
            fc = 255;
        }
        if (bc &gt; 255) {
            bc = 255;
        }
        int r = fc + random.nextInt(bc - fc);
        int g = fc + random.nextInt(bc - fc);
        int b = fc + random.nextInt(bc - fc);
        return new Color(r, g, b);
    }

}
</pre>

* * *

&nbsp;

## index.jsp表单

<pre class="lang:default decode:true">&lt;%--
  Created by IntelliJ IDEA.
  User: 小信
  Date: 2018/8/10
  Time: 7:45
  To change this template use File | Settings | File Templates.
--%&gt;
&lt;%@ page contentType="text/html;charset=UTF-8" language="java" %&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;$Title$&lt;/title&gt;
&lt;/head&gt;
&lt;script type="text/javascript"&gt;
    function change() {
        document.getElementById("im").src = "${pageContext.request.contextPath}/tool?time"
            + new Date().getTime();
    };
&lt;/script&gt;
&lt;body&gt;
${requestScope["regist.message"] }
&lt;br/&gt;
&lt;br/&gt;
&lt;br/&gt;
&lt;form action="${pageContext.request.contextPath }/validation" method="post"&gt;
    &lt;input type="text" name="checkcode" id="checkcode"&gt;
    &lt;input type="submit" value="验证"/&gt;
&lt;/form&gt;
&lt;img
        src='${pageContext.request.contextPath}/tool' id="im"
        onclick="change();"&gt;&lt;span id="checkcode_span"&gt;&lt;a
        href="javascript:void(0)" onclick="change();"&gt;&lt;font
        color='black'&gt;换一张&lt;/font&gt;&lt;/a&gt;&lt;br/&gt;&lt;/span&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>

* * *


## web.xml配置

<pre class="lang:default decode:true">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"&gt;
    &lt;servlet&gt;
        &lt;servlet-name&gt;imgtool&lt;/servlet-name&gt;
        &lt;servlet-class&gt;xyz.xioaxin12.utils.ImgToolServlet&lt;/servlet-class&gt;
    &lt;/servlet&gt;
    &lt;servlet&gt;
        &lt;servlet-name&gt;ValidationServlet&lt;/servlet-name&gt;
        &lt;servlet-class&gt;xyz.xioaxin12.dao.ValidationServlet&lt;/servlet-class&gt;
    &lt;/servlet&gt;
    &lt;servlet-mapping&gt;
        &lt;servlet-name&gt;ValidationServlet&lt;/servlet-name&gt;
        &lt;url-pattern&gt;/validation&lt;/url-pattern&gt;
    &lt;/servlet-mapping&gt;
    &lt;servlet-mapping&gt;
        &lt;servlet-name&gt;imgtool&lt;/servlet-name&gt;
        &lt;url-pattern&gt;/tool&lt;/url-pattern&gt;
    &lt;/servlet-mapping&gt;
&lt;/web-app&gt;</pre>

* * *

&nbsp;

## 实现检验验证码，错误输出错误信息，重新回到本页面。正确则转发到login.jsp页面。

<pre class="lang:java decode:true">package xyz.xioaxin12.dao;

import ...

public class ValidationServlet extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request,response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //解决乱码
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html; charset=utf-8");

        // 验证码操作
        String checkcode = request.getParameter("checkcode");

        String _checkcode = (String) request.getSession().getAttribute(
                "checkcode_session");
        System.out.println("checkcode--" + checkcode);
        System.out.println("checkcode_session--" + _checkcode);
        request.getSession().removeAttribute("checkcode_session");
        if (_checkcode == null || (!_checkcode.equals(checkcode))) {
            request.setAttribute("regist.message", "验证码不正确");
            request.getRequestDispatcher("/index.jsp").forward(request,
                    response);
            return;
        }
        request.getRequestDispatcher("/login.jsp").forward(request,response);
    }
}
</pre>
&nbsp;

如图：

![](http://image.xiaoxinyes.club/2018_8_13.gif)
