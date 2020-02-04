---
title: '[JAVA]逻辑分页与物理分页'
date: 2020-01-05 13:49:29
categories: JAVA
tags: mybatis
---

code：mybatis+springboot

## 解释
### 逻辑分页
**逻辑分页并不是返回分页结果，而是直接返回全部数据，再通过代码获取分页数据。**

### 物理分页
**使用mysql的limit关键字，直接返回分页结果。**

---

## 编码
### sql
```
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for student
-- ----------------------------
DROP TABLE IF EXISTS `student`;
CREATE TABLE `student`  (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 7 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci ROW_FORMAT = Compact;

-- ----------------------------
-- Records of student
-- ----------------------------
INSERT INTO `student` VALUES (1, '测试1', 11);
INSERT INTO `student` VALUES (2, '测试2', 22);
INSERT INTO `student` VALUES (3, '测试3', 14);
INSERT INTO `student` VALUES (4, '测试4', 16);
INSERT INTO `student` VALUES (5, '测试5', 32);
INSERT INTO `student` VALUES (6, '测试6', 23);

SET FOREIGN_KEY_CHECKS = 1;
```

### 对应的实体类
```
public class Student {
    //构造
    private int id;
    private String name;
    private int age;
    //get set
}
```

### pom.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.2.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>mybatis</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>mybatis</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.1</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <build>
        <!--注意：如果xml文件不放到resources下，那么需要配置以下代码，不然target下找不到-->
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.*</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.yml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
        </resources>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

### application.properties
```
#jdbc驱动
spring.datasource.driverClassName = com.mysql.cj.jdbc.Driver
spring.datasource.url = jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai
spring.datasource.username = ****
spring.datasource.password = ****

#mybatis
#xml地址
mybatis.mapper-locations=classpath:com/example/mybatis/dao/mapping/**.xml
#控制台打印sql
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

## 结构
> Controller --> service --> dao -->xml

### controller
```
@RestController
public class StudentController {

    @Autowired
    private SutndentServiceImpl studentService;

    /**
     * 物理分页
     */
    @GetMapping("physical-paging")
    public Object getList1(@RequestParam(value = "start", required = false, defaultValue = "1") int start,
                           @RequestParam(value = "limit", required = false, defaultValue = "3") int limit) {

        List<Student> list = studentService.physicalPaging(start, limit);
        return list;

    }

    /**
     * 逻辑分页
     */
    @GetMapping("logical-paging")
    public Object getList2(@RequestParam(value = "start", required = false, defaultValue = "1") int start,
                           @RequestParam(value = "limit", required = false, defaultValue = "3") int limit) {

        List<Student> list = studentService.logicalPaging(start, limit);
        return list;

    }
}
```
### service+实现类
```
public interface StudentService {
    /**
     * 物理分页
     *
     * @param start 起始页
     * @param limit 每页条数
     * @return
     */
    List<Student> physicalPaging(int start, int limit);

    /**
     * 逻辑分页
     *
     * @param start
     * @param limit
     * @return
     */
    List<Student> logicalPaging(int start, int limit);
}
```

注意：实现类的@Service
```
@Service
public class SutndentServiceImpl implements StudentService {
    @Autowired
    private StudentDao studentDao;

    @Override
    public List<Student> physicalPaging(int start, int limit) {
        //这里是分页的关键
        if (start < 1) {
            start = 1;
        } else {
            start = (start - 1) * limit;
        }
        List<Student> list = studentDao.getList(start, limit);
        return list;
    }

    @Override
    public List<Student> logicalPaging(int start, int limit) {
        if (start < 1) {
            start = 1;
        }
        List<Student> list = studentDao.getAllList();
        if((start-1)*limit > list.size()){
            return Collections.emptyList();
        }
        return list.subList((start - 1) * limit, ((start * limit) > list.size() ? list.size() : (start * limit)));
    }
}

```

### dao
注意：类上的@Mapper,文件多时可选择启动类上设置@MapperScan，这样不需要每个mapper加注解。
```
@Mapper
public interface StudentDao {
    /**
     * 注意:1、有@Mapper注解  2、一个以上的参数需要加@param
     *
     * @param start 起始页
     * @param limit 每页多少条
     * @return
     */
    List<Student> getList(@Param("start") int start, @Param("limit") int limit);

    /**
     * 获取全部数据
     *
     * @return
     */
    List<Student> getAllList();
}

```

### XML映射文件
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.mybatis.dao.StudentDao">

    <!-- 通用查询映射结果 -->
    <resultMap id="BaseResultMap" type="com.example.mybatis.baen.Student">
        <id column="id" property="id"/>
        <result column="name" property="name"/>
        <result column="age" property="age"/>
    </resultMap>

    <!-- 通用查询结果列 -->
    <sql id="Base_Column_List">
        id, name, age
    </sql>
    <select id="getList" resultType="com.example.mybatis.baen.Student">
        select
        <include refid="Base_Column_List"/>
        from student limit #{start},#{limit}
    </select>

    <select id="getAllList" resultType="com.example.mybatis.baen.Student">
        select
        <include refid="Base_Column_List"/>
        from student
    </select>

</mapper>
```

## 测试
### 逻辑分页接口测试
```
http://127.0.0.1:8080/logical-paging?start=1&limit=2
---
[
    {
        "id": 1,
        "name": "测试1",
        "age": 11
    },
    {
        "id": 2,
        "name": "测试2",
        "age": 22
    }
]
```

### 物理分页接口测试
```
http://127.0.0.1:8080/physical-paging?start=1&limit=2
---
[
    {
        "id": 1,
        "name": "测试1",
        "age": 11
    },
    {
        "id": 2,
        "name": "测试2",
        "age": 22
    }
]
```