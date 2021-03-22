---
title: '[JAVA]-视频url获取指定帧数的图片'
date: 2021-03-17 16:00:25
categories: JAVA
tags: java
---

**通过视频地址，获取指定N帧的图片，并且上传到阿里云oss**

参考地址：http://www.fixbbs.com/p/01439272.html

## 依赖
```
<dependency>
    <groupId>org.bytedeco</groupId>
    <artifactId>javacv</artifactId>
    <version>1.4.3</version>
</dependency>
<dependency>
    <groupId>org.bytedeco.javacpp-presets</groupId>
    <artifactId>ffmpeg-platform</artifactId>
    <version>4.0.2-1.4.3</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>RELEASE</version>
</dependency>
```

## 相关代码
```
    @Override
    public Response upload(MultipartFile file) {
        if (file == null) {
            return Response.fail().message("缺少必要参数！");
        }

        //获取文件名加后缀
        String fileName = file.getOriginalFilename();
        if (fileName == null) {
            return Response.fail().message("上传异常！");
        }
        //获取文件后缀
        String suffix = "";

        int index = fileName.lastIndexOf(".");
        if (index > 0) {
            suffix = file.getOriginalFilename().substring(index, fileName.length());
        }

        String contentType = file.getContentType();
        String resources = ALYConstants.OSS_PUBLIC_PICTURE + "other/";
        if (contentType != null) {
            if (contentType.contains("video")) {
                resources = ALYConstants.OSS_PUBLIC_PICTURE + "video/";
            } else if (contentType.contains("image")) {
                resources = ALYConstants.OSS_PUBLIC_PICTURE + "image/";
                if (StringUtils.isBlank(suffix)) {
                    suffix = ".jpg";
                }
            }
        }

        //组合新的文件名
        String imgName = DateUtils.dateTimeNow() + "-" + IdUtils.fastSimpleUUID() + suffix;

        resources = resources + DateUtils.dateTime() + "/";

        try {
            boolean state = this.oss(file.getBytes(), resources + imgName);
            if (!state) {
                return Response.fail().message("文件上传失败！");
            }
        } catch (IOException e) {
            log.error("【oss上传资源异常，异常信息：[{}]】", e.getMessage());
        }
        String url = ALYConstants.OSS_STATIC_MAIN_LINK + resources + imgName;

        this.insert(file, url, imgName, suffix, contentType);

        return Response.ok().content(new JSONObject() {{
            put("url", url);
        }});
    }

    @Override
    public String videoToImage(String url) throws Exception {
        String targetFilePath = "";
        FFmpegFrameGrabber grabber = FFmpegFrameGrabber.createDefault(url);
        grabber.start();

        //判断是否是竖屏小视频
        String rotate = grabber.getVideoMetadata("rotate");
        int lengthInFrames = grabber.getLengthInFrames();

        Frame frame;

        int i = 0;
        int index = 3;//截取图片第几帧
        while (i < lengthInFrames) {
            frame = grabber.grabImage();
            if (i == index) {
                if (null != rotate && rotate.length() > 1) {
                    targetFilePath = doExecuteFrame(frame, true);   //获取缩略图
                } else {
                    targetFilePath = doExecuteFrame(frame, false);   //获取缩略图
                }
                break;
            }
            i++;
        }

        grabber.stop();

        return targetFilePath;  //返回的是视频第N帧
    }

    private void insert(MultipartFile file, String url, String newName, String suffix, String contentType) {
        String memberId = memberService.infoId();

        CpFile cpFile = new CpFile();
        cpFile.setId(IdUtils.getSnowFlakeId());
        cpFile.setMemberId(memberId);
        cpFile.setName(file.getOriginalFilename());
        cpFile.setNewName(newName);
        cpFile.setUrl(url);
        cpFile.setType(contentType);
        cpFile.setSize(file.getSize());
        cpFile.setSuffix(suffix);
        cpFile.setCreateTime(DateUtils.getNowDate());
        this.baseMapper.insert(cpFile);

        log.info("【文件上传，上传用户：[{}]，地址：[{}]】", memberId, url);

    }

    private boolean oss(byte[] file, String name) {
        String oss = ALYConstants.OSS_STATIC_POSITION;
        String endpoint = ALYConstants.OSS_ENDPOINT;
        String accessKeyId = ALYConstants.OSS_ACCESS_KEY;
        String accessKeySecret = ALYConstants.OSS_ACCESS_SECRET;
        try {
            //创建OSSClient实例。
            OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
            //传Byte数组。
            ossClient.putObject(oss, name, new ByteArrayInputStream(file));
            //关闭OSSClient。
            ossClient.shutdown();

            return true;
        } catch (Exception e) {
            log.error("【文件上传失败，错误信息：[{}]】", e.getMessage());
        }

        return false;
    }

    /**
     * 截取缩略图，存入阿里云OSS（按自己的上传类型自定义转换文件格式）
     *
     * @return 图片地址
     * @throws Exception
     */
    public String doExecuteFrame(Frame f, boolean direction) throws Exception {
        if (null == f || null == f.image) {
            return "";
        }

        Java2DFrameConverter converter = new Java2DFrameConverter();
        BufferedImage bi = converter.getBufferedImage(f);
        if (direction) {
            Image image = (Image) bi;
            bi = rotate(image, 90);//图片旋转90度
        }
        ByteArrayOutputStream os = new ByteArrayOutputStream();
        ImageIO.write(bi, "png", os);

        InputStream input = new ByteArrayInputStream(os.toByteArray());
        MultipartFile multipartFile = new MockMultipartFile("temp.jpg", "temp.jpg", "temp.jpg", input);

        Response data = this.upload(multipartFile);
        Object content = data.getContent();

        return JSONObject.parseObject(String.valueOf(content)).getString("url");
    }

    /**
     * 图片旋转角度
     *
     * @param src   源图片
     * @param angel 角度
     * @return 目标图片
     */
    public static BufferedImage rotate(Image src, int angel) {
        int src_width = src.getWidth(null);
        int src_height = src.getHeight(null);

        // calculate the new image size
        Rectangle rectangle = CalcRotatedSize(new Rectangle(new Dimension(
                src_width, src_height)), angel);

        BufferedImage res;
        res = new BufferedImage(rectangle.width, rectangle.height,
                BufferedImage.TYPE_INT_RGB);
        Graphics2D g2 = res.createGraphics();

        // transform(这里先平移、再旋转比较方便处理；绘图时会采用这些变化，绘图默认从画布的左上顶点开始绘画，源图片的左上顶点与画布左上顶点对齐，然后开始绘画，修改坐标原点后，绘画对应的画布起始点改变，起到平移的效果；然后旋转图片即可)
        //平移（原理修改坐标系原点，绘图起点变了，起到了平移的效果，如果作用于旋转，则为旋转中心点）
        g2.translate((rectangle.width - src_width) / 2, (rectangle.height - src_height) / 2);
        // 旋转（原理transalte(dx,dy)->rotate(radians)->transalte(-dx,-dy);修改坐标系原点后，旋转90度，然后再还原坐标系原点为(0,0),但是整个坐标系已经旋转了相应的度数 ）
        g2.rotate(Math.toRadians(angel), src_width / 2, src_height / 2);
        // 先旋转（以目标区域中心点为旋转中心点，源图片左上顶点对准目标区域中心点，然后旋转）
        // g2.translate(rect_des.width/2,rect_des.height/ 2);
        // g2.rotate(Math.toRadians(angel));
        // 再平移（原点恢复到源图的左上顶点处（现在的右上顶点处），否则只能画出1/4）
        // g2.translate(-src_width/2,-src_height/2);
        g2.drawImage(src, null, null);

        return res;
    }

    /**
     * 计算转换后目标矩形的宽高
     *
     * @param src   源矩形
     * @param angel 角度
     * @return 目标矩形
     */
    private static Rectangle CalcRotatedSize(Rectangle src, int angel) {
        double cos = Math.abs(Math.cos(Math.toRadians(angel)));
        double sin = Math.abs(Math.sin(Math.toRadians(angel)));
        int des_width = (int) (src.width * cos) + (int) (src.height * sin);
        int des_height = (int) (src.height * cos) + (int) (src.width * sin);

        return new java.awt.Rectangle(new Dimension(des_width, des_height));
    }
```