---
title: '[JAVA]easyexcel导出'
date: 2020-02-26 21:29:59
categories: JAVA
tags: java
---

阿里的表格工具，简单了解了一下。

## 依赖
```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>easyexcel</artifactId>
    <version>2.1.2</version>
</dependency>
```

---

## 新增一个实体类
```
@Data
@ContentRowHeight()
@HeadRowHeight()
@ColumnWidth()
public class Student {
    @ExcelProperty("学生姓名")
    private String name;
    @ExcelProperty("学生年龄")
    private int age;
    @ExcelProperty(value = "性别")
    private String sex;
    @DateTimeFormat("yyyy年MM月dd日HH时mm分ss秒")
    @ExcelProperty(value = "日期",index = 5)
    private Date date;
}
```

**解释：**

@Data：lombok的注解，可以省略@get/@set等等。

@ContentRowHeight()/@HeadRowHeight()/@ColumnWidth()：可修改列宽、行高。

@ExcelProperty：定义列名称或者index位置，默认0开始。

@DateTimeFormat：日期转换

---

## ExcelTest.java
```
@RunWith(SpringRunner.class)
@SpringBootTest
public class ExcelTest {
    /**
     * 最简单的导出
     */
    @Test
    public void simpleWrite01() throws IOException {
        // 写法1
        String fileName = (new File("").getCanonicalPath()) + "/simpleWrite" + System.currentTimeMillis() + ".xlsx";
        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
        // 如果这里想使用03 则 传入excelType参数即可
        EasyExcel.write(fileName, Student.class).sheet("模板").doWrite(getData());

        // 写法2
/*        fileName = (new File("").getCanonicalPath()) + "/simpleWrite" + System.currentTimeMillis() + ".xlsx";
        // 这里 需要指定写用哪个class去写
        ExcelWriter excelWriter = EasyExcel.write(fileName, Student.class).build();
        WriteSheet writeSheet = EasyExcel.writerSheet("模板").build();
        excelWriter.write(getData(), writeSheet);
        // 千万别忘记finish 会帮忙关闭流
        excelWriter.finish();
*/
    }

    /**
     * 指定列名导出
     */
    @Test
    public void simpleWrite02() throws IOException {
        String fileName = (new File("").getCanonicalPath()) + "/simpleWrite" + System.currentTimeMillis() + ".xlsx";
        // 根据用户传入字段 假设我们要忽略 date
        Set<String> columnFiledNames = new HashSet<String>();
        columnFiledNames.add("age");
        columnFiledNames.add("sex");
        //1、需要忽略的列名
//        EasyExcel.write(fileName, Student.class).excludeColumnFiledNames(columnFiledNames).sheet("模板").doWrite(getData());
        //2、需要保留的列名
        EasyExcel.write(fileName, Student.class).includeColumnFiledNames(columnFiledNames).sheet("模板").doWrite(getData());

    }



    private List<Student> getData() {
        List<Student> list = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            Student student = new Student();
            student.setName("小信" + i);
            student.setAge(22);
            student.setSex("男");
            student.setDate(new Date());
            list.add(student);
        }
        return list;
    }
}

```

---

## web下载导出
```
@RestController
public class WebExcelTest {
    /**
     * web中写入
     */
    @GetMapping("get")
    public void getExcel(HttpServletResponse response) {

        try {
            response.setContentType("application/vnd.ms-excel");
            response.setCharacterEncoding("utf-8");
            response.setHeader("Content-disposition", "attachment;filename=" + System.currentTimeMillis() + ".xlsx");
            // 这里需要设置不关闭流
            EasyExcel.write(response.getOutputStream(), Student.class).autoCloseStream(Boolean.FALSE).sheet("模板")
                    .doWrite(getData());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }


    private List<Student> getData() {
        List<Student> list = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            Student student = new Student();
            student.setName("小信" + i);
            student.setAge(22);
            student.setSex("男");
            student.setDate(new Date());
            list.add(student);
        }
        return list;
    }
```

---


**官方文档：[点击](https://alibaba-easyexcel.github.io/quickstart/write.html)**