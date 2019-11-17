---
title: Mybatis注解方式操作表数据
date: 2019-11-17 12:00:07
categories: JAVA
tags: mybatis
---

# springboot+mybaits演示
@Insert/@Delete/@Select/@Update

---
## 一、依赖
```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.0.0</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.25</version>
        </dependency>
```

## 二、启动类
```
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication app = new SpringApplication(DemoApplication.class);
        app.run(args);
    }
}
```

## 三、application,yml
```
spring:
  datasource:
    password: 1579886070
    username: root
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8&useSSL=false
    
mybatis:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl #打印sql
```
## 四、student数据表
```
CREATE TABLE `student` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=22 DEFAULT CHARSET=utf8;
```
## 五、来一个实体类
```
public class Student {

    public Student() {
    }

    public Student(Integer id, String name, Integer age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    private Integer id;
    private String name;
    private Integer age;
    
    //get
    //set
}
```

## 六、service和它的实现类
```
public interface StudentService {
    int add(Student student);

    boolean deleteById(int id);

    List<Student> selectAll();

    boolean updateStudent(Student s);
}
```
```
@Service
public class StudentServiceImlp implements StudentService {

    @Autowired
    private StudentMapper studentMapper;

    @Override
    public int add(Student student) {
        return studentMapper.insertStudent(student);
    }

    @Override
    public boolean deleteById(int id) {

        return studentMapper.deleteStudent(id);
    }

    @Override
    public List<Student> selectAll() {
        return studentMapper.selectStudentAll();
    }

    @Override
    public boolean updateStudent(Student s) {
        return studentMapper.update(s);
    }
}
```

## 七、注入mapper
包含增删改查，类上加了@mapper,则不用再启动类上@MapperScan()
```
@Mapper
public interface StudentMapper {
    @Insert("insert into student(name,age) values (#{name},#{age})")
    int insertStudent(Student student);

    @Delete("delete from student where id = #{id}")
    boolean deleteStudent(int id);

    @Select("select * from student")
    List<Student> selectStudentAll();

    @Update("update student set name=#{name},age = #{age} where id = #{id}")
    boolean update(Student student);
}
```

## 八、最后controller
```
@RestController
@RequestMapping
public class StudentController {
    @Autowired
    private StudentService studentService;

    /**
     * 添加数据
     *
     * @param name
     * @param age
     * @return
     */
    @GetMapping("addStu")
    public Object insertStudent(@Param("name") String name, @Param("age") int age) {
        Student student = new Student(null, name, age);
        int index = studentService.add(student);
        return index;
    }

    /**
     * 根据id删除数据
     *
     * @param id
     * @return
     */
    @GetMapping("delete")
    public Object delete(@Param("id") int id) {
        return studentService.deleteById(id);
    }

    /**
     * 查询所有数据
     *
     * @return
     */
    @GetMapping("select")
    public Object select() {
        return studentService.selectAll();
    }

    /**
     * 修改数据
     *
     * @param id
     * @param name
     * @param age
     * @return
     */
    @GetMapping("update")
    public Object update(int id, String name, int age) {
        Student s = new Student(id, name, age);
        return studentService.updateStudent(s);
    }
}
```
