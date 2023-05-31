---
title: MyBatis-Plus使用
date: 2022-05-20 08:46:09
tags:
- JavaEE
- MyBatis
- 后端
categories: Java
cover: https://s2.loli.net/2022/05/16/pDs5EMFkXvHJyZ1.png
---
# MyBatis-Plus简介

**本次将使用SpringBoot进行案例演示**

**学习本章节之前，最好先有[Mybatis基础](/docs/2022/05/16/MyBatis使用/)以及SpringBoot基础**

## 1.简介
**MyBatis-Plus**（简称 MP）是一个 **MyBatis的增强工具**，在 MyBatis 的基础上**只做增强不做改变**，为
**简化开发、提高效率而生**。

## 2.特性
- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作
## 3.框架结构
![Mybatis-Plus框架结构](Snipaste_2022-05-16_14-28-07.png)

# 创建项目并初始化开发环境
- IDE: IDEA 2021.3
- JDK: jdk8+
- 构建工具: Maven3.5.4
- 数据库版本: Mysql8.0.28
- SpringBoot: 2.5.12
- MyBatis-Plus: 3.5.1

## 创建数据库表
```sql
CREATE DATABASE `mybatis_plus`
USE `mybatis_plus`;
CREATE TABLE `user` (
  `id` bigint(20) NOT NULL COMMENT '主键ID',
  `name` varchar(30) DEFAULT NULL COMMENT '姓名',
  `age` int(11) DEFAULT NULL COMMENT '年龄',
  `email` varchar(50) DEFAULT NULL COMMENT '邮箱',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

## 添加数据

```mysql
INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```

## 引入依赖包

```xml
  <dependencies>
    <# SpringBoot启动器 #>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
    <# MybatisPlus依赖包 #>
    <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter</artifactId>
      <version>3.5.1</version>
    </dependency>
    <# Lombok依赖包 #>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <optional>true</optional>
    </dependency>
    <# Mysql依赖包 #>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <scope>runtime</scope>
    </dependency>
  </dependencies>
```

# 编写代码

## 配置application.yml

```yaml
spring:
    # 配置数据源信息
    datasource:
        # 配置数据源类型
        type: com.zaxxer.hikari.HikariDataSource
        # 配置连接数据库信息
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://localhost:3306/mybatis_plus?characterEncoding=utf-8&useSSL=false
        username: root
        password: 123456
```

## 创建实体类

```java
// User.Java

@Data // Lombok注解生成属性的get set方法
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

## 添加mapper

> BaseMapper是MyBatis-Plus提供的模板mapper，其中包含了基本的CRUD方法，泛型为操作的
> 实体类型

```java
public interface UserMapper extends BaseMapper<User> {
}
```

## 启动类

> 在Spring Boot启动类中添加 `@MapperScan` 注解，扫描mapper包

```java
// 扫描Mapper接口的所在地
@MapperScan("com.along.mybatisplus.mapper")
public class MybaitsPlusApplication {
    public static void main(String[] args) {
        SpringApplication.run(MybaitsPlusApplication.class, args);
    }
}
```

## 测试数据

```java
@SpringBootTest
public class MybatisPlusTest {
    @Autowired
    private UserMapper userMapper;

    @Test
    public void testSelectList() {
        //selectList()根据MP内置的条件构造器查询一个list集合，null表示没有条件，即查询所有
        List<User> lists = userMapper.selectList(null);
        lists.forEach(System.out::println);
    }
}
```

> IDEA在 UserMapper 处报错，因为找不到注入的对象，因为类是动态创建的，但是程序可以正确的执行。
> 为了避免报错，可以在mapper接口上添加 `@Repository` 注解

## 添加日志

```yaml
# 配置Mybatis日志
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

# 基本CURD

 ## BaseMapper结构

```java
public interface BaseMapper<T> extends Mapper<T> {
    /**
     * 插入一条记录
     *
     * @param entity 实体对象
     */
    int insert(T entity);

    /**
     * 根据 ID 删除
     *
     * @param id 主键ID
     */
    int deleteById(Serializable id);

    /**
     * 根据实体(ID)删除
     *
     * @param entity 实体对象
     * @since 3.4.4
     */
    int deleteById(T entity);

    /**
     * 根据 columnMap 条件，删除记录
     *
     * @param columnMap 表字段 map 对象
     */
    int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);

    /**
     * 根据 entity 条件，删除记录
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where
     *                     语句）
     */
    int delete(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 删除（根据ID 批量删除）
     *
     * @param idList 主键ID列表(不能为 null 以及 empty)
     */
    int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);

    /**
     * 根据 ID 修改
     *
     * @param entity 实体对象
     */
    int updateById(@Param(Constants.ENTITY) T entity);

    /**
     * 根据 whereEntity 条件，更新记录
     *
     * @param entity        实体对象 (set 条件值,可以为 null)
     * @param updateWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成
     *                      where 语句）
     */
    int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> updateWrapper);

    /**
     * 根据 ID 查询
     *
     * @param id 主键ID
     */
    T selectById(Serializable id);

    /**
     * 查询（根据ID 批量查询）
     *
     * @param idList 主键ID列表(不能为 null 以及 empty)
     */
    List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);

    /**
     * 查询（根据 columnMap 条件）
     *
     * @param columnMap 表字段 map 对象
     */
    List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);

    /*** 根据 entity 条件，查询一条记录
     * <p>查询一条记录，例如 qw.last("limit 1") 限制取一条记录, 注意：多条数据会报异常
     </p>
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    default T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper) {
        List<T> ts = this.selectList(queryWrapper);
        if (CollectionUtils.isNotEmpty(ts)) {
            if (ts.size() != 1) {
                throw ExceptionUtils.mpe("One record is expected, but the query result is multiple records");
            }
            return ts.get(0);
        }
        return null;
    }

    /**
     * 根据 Wrapper 条件，查询总记录数
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    Long selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 entity 条件，查询全部记录
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 Wrapper 条件，查询全部记录
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T>
                                                 queryWrapper);

    /**
     * 根据 Wrapper 条件，查询全部记录
     * <p>注意： 只返回第一个字段的值</p>
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 entity 条件，查询全部记录（并翻页）
     *
     * @param page         分页查询条件（可以为 RowBounds.DEFAULT）
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    <P extends IPage<T>> P selectPage(P page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 Wrapper 条件，查询全部记录（并翻页）
     *
     * @param page         分页查询条件
     * @param queryWrapper 实体对象封装操作类
     */
    <P extends IPage<Map<String, Object>>> P selectMapsPage(P page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
}
```

## 新增数据

```java
    @Test
    public void testInsert() {
        // 实现新增用户信息
        User user = new User();
        user.setName("张三");
        user.setAge(23);
        user.setEmail("zhangsan@qq.com");
        int result = userMapper.insert(user);
        System.out.println("result：" + result);
    }
```

## 删除数据（不使用条件构造器）

```java
    public void testDelete() {
        // 通过Id删除用户信息
        int result = userMapper.deleteById(1526364090026594306L);
         System.out.println("result：" + result);

        // 通过Map删除
         Map<String, Object> map = new HashMap<>();
         map.put("name", "张三");
         map.put("age", 23);
         int result = userMapper.deleteByMap(map);
         System.out.println("result：" + result);

        // 通过Collection
        List<Long> longs = Arrays.asList(1L, 2L, 3L);
        int result = userMapper.deleteBatchIds(longs);
        System.out.println("result：" + result);
    }
```

## 修改数据（不适用条件构造器）

```java
    @Test
    public void testUpdate() {
        // 根据Id修改用户信息
        User user = new User();
        user.setId(4L);
        user.setName("Sandy");
        int result = userMapper.updateById(user);
        System.out.println("result：" + result);
    }
```

## 查询数据（不使用条件构造器）

```java
    @Test
    public void testSelect() {
        // 通过Id查询信息
        // SELECT id,name,age,email FROM user WHERE id=?
        User user = userMapper.selectById(1L);
        System.out.println(user);

        // 根据多个Id查询多个用户信息
        // SELECT id,name,age,email FROM user WHERE id IN ( ? , ? , ? )
        List<Long> longs = Arrays.asList(1L, 2L, 3L);
        List<User> users = userMapper.selectBatchIds(longs);
        System.out.println(users);

        // 根据Map集合查询用户信息
        // SELECT id,name,age,email FROM user WHERE name = ? AND age = ?
        Map<String, Object> map = new HashMap<>();
        map.put("name", "Jack");
        map.put("age", 20);
        List<User> users = userMapper.selectByMap(map);
        users.forEach(System.out::println);

        // 查询所有数据
        // SELECT id,name,age,email FROM user
        List<User> users = userMapper.selectList(null);
        users.forEach(System.out::println);
    }
```

---

## IService

> MyBatis-Plus中有一个接口 `IService` 和其实现类 `ServiceImpl`，封装了常见的业务层逻辑
>
> 详情查看源码 `IService` 和 `ServiceImpl`

## 创建Service以及实现类

```java
// UserService.java 接口

/**
* UserService继承IService模板提供的基础功能
*/
public interface UserService extends IService<User> {}

```

添加 `@Service`注册为Spring组件

```java
// UserService.java 实现类

/**
* ServiceImpl实现了IService，提供了IService中基础功能的实现
* 若ServiceImpl无法满足业务需求，则可以使用自定的UserService定义方法，并在实现类中实现
*/
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService{}
```

## 测试查询总条数

```java
    @Test
    public void testGetCount(){
        // 查询总记录数
        // SELECT COUNT( * ) FROM user
        long count = userService.count();
        System.out.println("总记录数：" + count);
    }
```

## 测试批量插入

```java
    @Test
    public void testInsertMore(){
        // 批量添加
        List<User> lists = new ArrayList<>();
        for (int i = 0; i < 5; i++) {
            User user = new User();
            user.setName("test" + i);
            lists.add(user);
        }
        // INSERT INTO user ( id, name ) VALUES ( ?, ? )
        boolean b = userService.saveBatch(lists);
        System.out.println(b);
    }
```

# Mybatis中常用的注解

## @TableName

> 经过以上的测试，在使用MyBatis-Plus实现基本的CRUD时，我们并没有指定要操作的表，只是在
> Mapper接口继承BaseMapper时，设置了泛型User，而操作的表为user表
> 由此得出结论，MyBatis-Plus在确定操作的表时，由BaseMapper的泛型决定，即实体类型决
> 定，且默认操作的表名和实体类型的类名一致

> 若实体类类型的类名和**要操作的表的表名不一**致，会出现什么问题？
>
> 我们将表user更名为t_user，测试查询功能
> 程序抛出异常，Table 'mybatis_plus.user' doesn't exist，因为现在的表名为t_user，而默认操作
> 的表名和实体类型的类名一致，即user表

### 通过@TableName解决

> 在实体类User.java 中添加上注解 `@TableName("t_user")` 标识实体类对应的表，就可以成功执行SQL语句

```java
@Data
// 设置实体类对应的表名
@TableName("t_user")
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

### MybatisPlus全局配置也可以解决

> 在开发的过程中，我们经常遇到以上的问题，即实体类所对应的表都有固定的前缀，例如t_或tbl_
> 此时，可以使用MyBatis-Plus提供的全局配置，为实体类所对应的表名设置默认的前缀，那么就
> 不需要在每个实体类上通过 `@TableName` 标识实体类对应的表

```yml
  # 设置Mybatis-Plus的全局配置
  global-config:
    db-config:
      table-prefix: t_ # 表前缀
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

## @TableId

> MyBatis-Plus在实现CRUD时，会默认将id作为主键列，并在插入数据时，默认
> 基于雪花算法的策略生成id，如果使用了uid，cid等，MybatisPlus并不会自动识别，则会抛出异常：Field 'uid' doesn't have a default value

### 使用@TableId

> 在实体类中uid属性上通过 `@TableId` 将其标识为主键，即可成功执行SQL语

```java
@Data
// 设置实体类对应的表名
//@TableName("t_user")
public class User {
    @TableId
    private Long uid;
    private String name;
    private Integer age;
    private String email;
}
```

### @TableId中的value属性

> 若实体类中主键对应的属性为id，而表中表示主键的字段为uid，此时若只在属性id上添加注解
> `@tableId`，则抛出异常Unknown column 'id' in 'field list'，即MyBatis-Plus仍然会将id作为表的
> 主键操作，而表中表示主键的是字段uid
> 此时需要通过@TableId注解的value属性，指定表中的主键字段，`@TableId("uid")` 或
> `@TableId(value="uid")`

```java
@Data
// 设置实体类对应的表名
//@TableName("t_user")
public class User {
    @TableId("uid")
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

### TableId中的type属性

> type属性用来定义主键生成策略

**常用的主键策略：**

| 属性值                   | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| IdType.ASSIGN_ID（默认） | 基于雪花算法的策略生成数据id，与数据库id是否设置自增无关     |
| IdType.AUTO              | 使用数据库的自增策略，注意，该类型请确保数据库设置了id自增，否则无效 |

### 配置全局主键策略

```yaml
mybatis-plus:
  # 设置Mybatis-Plus的全局配置
  global-config:
    db-config:
      # 配置MyBatis-Plus操作表的默认前缀
      table-prefix: t_
      # 配置MyBatis-Plus的主键策略
      id-type: auto
  configuration:
    # 配置Mybatis日志
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

## @TableField

> MyBatis-Plus在执行SQL语句时，要保证实体类中的属性名和表中的字段名一致
> 如果实体类中的属性名和字段名不一致的情况，会出现什么问题呢？

### 情况1

> 若实体类中的属性使用的是驼峰命名风格，而表中的字段使用的是下划线命名风格
> 例如实体类属性userName，表中字段user_name
> 此时MyBatis-Plus会自动将下划线命名风格转化为驼峰命名风格
> 相当于在MyBatis中配置

### 情况2

> 若实体类中的属性和表中的字段不满足情况1
> 例如实体类属性name，表中字段username
> 此时需要在实体类属性上使用 `@TableField("username")` 设置属性所对应的字段名

```java
@Data
// 设置实体类对应的表名
//@TableName("t_user")
public class User {
    private Long id;
    @TableField("username")
    private String name;
    private Integer age;
    private String email;
}
```

## @TableLogic

### 各种删除

- **物理删除**：真实删除，将对应数据从数据库中删除，之后查询不到此条被删除的数据
- **逻辑删除**：假删除，将对应数据中代表是否被删除字段的状态修改为“被删除状态”，之后在数据库中仍旧能看到此条数据记录
- **使用场景**：可以进行数据恢复

### 实现逻辑删除

> 步骤1：在数据库表中新建一个逻辑删除队列，设置默认值为 0

![创建一个逻辑删除列](Snipaste_2022-05-17_14-18-08.png)

> 步骤2：实体类中添加逻辑删除属性并且加上 `@TableLogic`注解

```java
@Data
// 设置实体类对应的表名
//@TableName("t_user")
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
    @TableLogic
    private Integer isDeleted;
}
```

> 步骤3：测试
>
> 测试删除功能，真正执行的是修改
> UPDATE t_user SET is_deleted=1 WHERE id=? AND is_deleted=0
> 测试查询功能，被逻辑删除的数据默认不会被查询
> SELECT id,username AS name,age,email,is_deleted FROM t_user WHERE is_deleted=0

# 条件构造器以及常用的接口

## Wrapper是什么

![Wrapper类之间的关系](Snipaste_2022-05-17_14-22-42.png)

- Wrapper：条件构造抽象类
  - AbstractWrapper ： 用于查询条件封装，生成 sql 的 where 条件
    - QueryWrapper ： 查询条件封装
    - UpdateWrapper ： Update 条件封装
    - AbstractLambdaWrapper ： 使用Lambda 语法
      - LambdaQueryWrapper ：用于Lambda语法使用的查询Wrapper
      - LambdaUpdateWrapper ： Lambda 更新封装Wrappe

## QueryWrapper

### 查询条件

```java
    @Test
    public void testSelect() {
        // 用户名包含A，年龄20到30之间，邮箱信息不为空的用户信息
        // SELECT id,name,age,email,is_deleted FROM t_user WHERE is_deleted=0 AND (name LIKE ? AND age BETWEEN ? AND ? AND email IS NOT NULL)
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.like("name", "a")
                .between("age", 20, 30)
                .isNotNull("email");
        List<User> users = userMapper.selectList(wrapper);
        users.forEach(System.out::println);
    }
```

### 排序查询条件

```java
    @Test
    public void testSelectGroupBy() {
        // 查询用户信息，按照年龄的降序排序，若年龄相同，则按照id升序排序
        // SELECT id,name,age,email,is_deleted FROM t_user WHERE is_deleted=0 ORDER BY age DESC,id ASC
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.orderByDesc("age").orderByAsc("id");
        List<User> users = userMapper.selectList(queryWrapper);
        System.out.println(users);
    }
```

### 删除条件

```java
    @Test
    public void testDeleteUser() {
        // 删除邮箱地址为null的用户信息
        // UPDATE t_user SET is_deleted=1 WHERE is_deleted=0 AND (email IS NULL)
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.isNull("email");
        int result = userMapper.delete(queryWrapper);
        System.out.println(result);
    }
```

### 修改条件

```java
    @Test
    public void testUpdate1() {
        // 将年龄大于20并且用户名中包含有a 或者 邮箱为null的用户信息修改
        // UPDATE t_user SET name=?, email=? WHERE is_deleted=0 AND (age > ? AND name LIKE ? OR email IS NOT NULL)
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.gt("age", 20).like("name", "a").or().isNotNull("email");
        User user = new User();
        user.setName("小明");
        user.setEmail("liaoliao@qq.com");
        int update = userMapper.update(user, queryWrapper);
        System.out.println(update);
    }
```

### 修改条件（条件优先级）

```java
    @Test
    public void testUpdate2() {
        // 将用户名中包含有a并且（年龄大于20或邮箱为null）的用户信息修改
        // UPDATE t_user SET name=?, email=? WHERE is_deleted=0 AND (name LIKE ? AND (age > ? OR email IS NULL))
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.like("name", "a").and(i -> i.gt("age", 20).or().isNull("email"));
        User user = new User();
        user.setName("小红");
        user.setEmail("liaoliao@qq.com");
        int update = userMapper.update(user, queryWrapper);
        System.out.println(update);
    }
```

### 分字段查询条件

```java
    @Test
    public void testSelect2() {
        // 查询用户的用户名，年龄，邮箱
        // SELECT name,age,email FROM t_user WHERE is_deleted=0
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.select("name", "age", "email");
        List<Map<String, Object>> maps = userMapper.selectMaps(queryWrapper);
        System.out.println(maps);
    }
```

### 实现子查询

> 只是示范一下，因为这是一个没有意义的查询

```java
    @Test
    public void testSelectZi() {
        // 查询id小于等于100的用户信息
        // SELECT id,name,age,email,is_deleted FROM t_user WHERE is_deleted=0 AND (id IN (select id from t_user where id <= 100))
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.inSql("id", "select id from t_user where id <= 100");
        List<User> users = userMapper.selectList(queryWrapper);
        System.out.println(users);
    }
```

## UpdateWrapper

### 修改条件[# 修改条件（条件优先级）](和这个一样效果)

```java
    @Test
    public void test08() {
        // 将用户名中包含有a并且（年龄大于20或邮箱为null）的用户信息修改
        // UPDATE t_user SET name=?,email=? WHERE is_deleted=0 AND (name LIKE ? AND (age > ? OR email IS NULL))
        UpdateWrapper<User> updateWrapper = new UpdateWrapper<>();
        updateWrapper.like("name", "a").and(i -> i.gt("age", 20).or().isNull("email"));
        updateWrapper.set("name", "廖狗子").set("email", "abcde@liaoliao.com");
        int update = userMapper.update(null, updateWrapper);
        System.out.println(update);
    }
```

## 模拟开发中组装条件的情况

> 在真正开发的过程中，组装条件是常见的功能，而这些条件数据来源于用户输入，是可选的，因此我们在组装这些条件时，必须先判断用户是否选择了这些条件，若选择则需要组装该条件，若没有选择则一定不能组装，以免影响SQL执行的结果

### 思路一

> 这种写法麻烦，容易失误写错

```java
    @Test
    public void test09() {
        //SELECT id,username AS name,age,email,is_deleted FROM t_user WHERE (age >= ? AND age <= ?)
        String username = "";
        Integer ageBegin = 20;
        Integer ageEnd = 30;
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        if (StringUtils.isNotBlank(username)) {
            // isNotBlank判断某个字符串是否不为空,不为null,不为空白符
            queryWrapper.like("username", "a");
        }
        if (ageBegin != null) {
            queryWrapper.ge("age", ageBegin);
        }
        if (ageEnd != null) {
            queryWrapper.le("age", ageEnd);
        }
        List<User> users = userMapper.selectList(queryWrapper);
        users.forEach(System.out::println);
    }
```

### 思路二

> 们可以使用带condition参数的重载方法构建查询条件，简化代码的编写

```java
    @Test
    public void test10() {
        // SELECT id,name,age,email,is_deleted FROM t_user WHERE is_deleted=0 AND (age >= ? AND age <= ?)
        String username = "";
        Integer ageBegin = 20;
        Integer ageEnd = 30;
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.like(StringUtils.isNotBlank(username), "name", username)
                .ge(ageBegin != null, "age", ageBegin)
                .le(ageEnd != null, "age", ageEnd);
        List<User> users = userMapper.selectList(queryWrapper);
        users.forEach(System.out::println);
    }
```

## LambdaQueryWrapper

```java
    @Test
    public void test11() {
        String username = "";
        Integer ageBegin = null;
        Integer ageEnd = 30;
        LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.like(StringUtils.isNotBlank(username), User::getName, username)
                .ge(ageBegin != null, User::getAge, ageBegin)
                .le(ageEnd != null, User::getAge, ageEnd);
        List<User> users = userMapper.selectList(queryWrapper);
        users.forEach(System.out::println);
    }
```

## LambdaUpdateWrapper

```java
    @Test
    public void test12() {
        // 将用户名中包含有a并且（年龄大于20或邮箱为null）的用户信息修改
        // UPDATE t_user SET name=?,email=? WHERE is_deleted=0 AND (name LIKE ? AND (age > ? OR email IS NULL))
        LambdaUpdateWrapper<User> updateWrapper = new LambdaUpdateWrapper<>();
        updateWrapper.like(User::getName, "a").and(i -> i.gt(User::getAge, 20).or().isNull(User::getEmail));
        updateWrapper.set(User::getName, "廖狗子").set(User::getEmail, "abcde@liaoliao.com");
        int update = userMapper.update(null, updateWrapper);
        System.out.println(update);
    }
```

# 插件

## 分页插件

> MyBatis Plus自带分页插件，只要简单的配置即可实现分页功能

### 添加配置类

> 创建 `com.along.config` 包
>
> config中新建 MybatisPlusConfig.java 配置类
>
> 添加 `@Configuration` 注解
>
> > 可以将当时在Application中的 `@MapperScan` 包扫描器注解转移到配置类中

```java
@Configuration
// 扫描Mapper接口所在的包
@MapperScan(value = {"com.along.mybatisplus.mapper"})
public class MybatisPlusConfig {
    // 添加一个MybatisPlus拦截器
    // 添加@Bean注解交给Spring容器管理
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        // 拿到拦截器
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 添加上page分页插件 PaginationInnerInterceptor
        // 给分页插件设置一个默认数据库类型 DbType.MYSQL（这里我用是Mysql）或者是DbType.SQL_SERVER都可以
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        // 返回这个拦截
        return interceptor;
    }
}
```

### 测试分页

```java
    @Test
    public void testPate() {
        Page<User> page = new Page<>(1, 3);
        userMapper.selectPage(page, null);
        System.out.println(page.getRecords()); // 获取数据记录
        System.out.println("当前页码：" + page.getCurrent()); // 获取当前页码
        System.out.println("每页显示数量：" + page.getSize()); // 获取当前每页显示数量
        System.out.println("总页数：" + page.getPages()); // 获取总页数
        System.out.println("总记录数：" + page.getTotal()); // 获取总记录数
        System.out.println("是否有下一页：" + page.hasNext()); // 是否有下一页
        System.out.println("是否有上一页：" + page.hasPrevious()); // 是否有上一页
    }
```

>结果：
>
>[User(id=1, name=Jone, age=18, email=test1@baomidou.com, isDeleted=0), User(id=2, name=Jack, age=20, email=test2@baomidou.com, isDeleted=0), User(id=3, name=Tom, age=28, email=test3@baomidou.com, isDeleted=0)]
>当前页码：1
>每页显示数量：3
>总页数：4
>总记录数：10
>是否有下一页：true
>是否有上一页：false

## 自定义分页功能

### UserMapper类中自定义方法

```java
    /**
     * 通过年龄查询用户信息并分页
     * @param page Mybatis-Plus 提供的分页对象
     * @param age age
     * @return
     */
    Page<User> selectPageVo(@Param("page") Page<User> page, @Param("age") Integer age);
```

### UserMapper.xml中编写SQL

> 首先要在application.yml中配置类型别名

```yaml
mybatis-plus:
  # 配置类型别名对应的包
  type-aliases-package: com.along.mybatisplus.pojo 
```

> MybatisPlus会自动将包内所有的实体添加别名

```xml
<!--IPage<User> selectPageVo(Page<User> page, Integer age);-->
<select id="selectPageVo" resultType="User">
    select id, name, age, email
    from t_user
    where age > #{age}
</select>
```

# 乐观锁

## 场景

> 一件商品，成本价是80元，售价是100元。老板先是通知小李，说你去把商品价格增加50元。小李正在玩游戏，耽搁了一个小时。正好一个小时后，老板觉得商品价格增加到150元，价格太高，可能会影响销量。又通知小王，你把商品价格降低30元。
>
> 此时，小李和小王同时操作商品后台系统。小李操作的时候，系统先取出商品价格100元；小王也在操作，取出的商品价格也是100元。小李将价格加了50元，并将100+50=150元存入了数据库；小王将商品减了30元，并将100-30=70元存入了数据库。是的，如果没有锁，小李的操作就
> 完全被小王的覆盖了。
>
> 现在商品价格是70元，比成本价低10元。几分钟后，这个商品很快出售了1千多件商品，老板直接亏哭

## 乐观锁与悲观锁

> 上面的故事，如果是乐观锁，小王保存价格前，会检查下价格是否被人修改过了。如果被修改过
> 了，则重新取出的被修改后的价格，150元，这样他会将120元存入数据库。
>
> 如果是悲观锁，小李取出数据后，小王只能等小李操作完之后，才能对价格进行操作，也会保证
> 最终的价格是120元。

### 乐观锁实现流程

> 数据库中添加version字段
>
> 取出记录时，获取当前version
>
> ```mysq
> SELECT id,`name`,price,`version` FROM product WHERE id=1
> ```
>
> 更新时，version + 1，如果where语句中的version版本不对，则更新失败
>
> ```mysql
> UPDATE product SET price = price + 50, `version`=`version` + 1 WHERE id = 1 AND `version`= 1
> ```
>
> 

## 模拟修改冲突

### 数据库中添加商品表

```mysql
CREATE TABLE t_product
(
id BIGINT(20) NOT NULL COMMENT '主键ID',
NAME VARCHAR(30) NULL DEFAULT NULL COMMENT '商品名称',
price INT(11) DEFAULT 0 COMMENT '价格',
VERSION INT(11) DEFAULT 0 COMMENT '乐观锁版本号',
PRIMARY KEY (id)
);
```

### 添加数据

```mysql
INSERT INTO t_product (id, NAME, price) VALUES (1, '外星人笔记本', 100);
```

### 添加实体

```java
@Data
public class Product {
    private Long id;
    private String name;
    private Integer price;
    private Integer version;
}
```

### 添加Mapper

> ProductMapper.java
>
> 添加 `@Repository` 标识为持久层组件

```java
@Repository
public interface ProductMapper extends BaseMapper<Product> {
}
```

### 测试代码以及数据

```java
    @Test
    public void testProduct01() {
        //1、小李
        Product p1 = productMapper.selectById(1L);
        System.out.println("小李取出的价格：" + p1.getPrice());
        
        //2、小王
        Product p2 = productMapper.selectById(1L);
        System.out.println("小王取出的价格：" + p2.getPrice());
        
        //3、小李将价格加了50元，存入了数据库
        p1.setPrice(p1.getPrice() + 50);
        int result1 = productMapper.updateById(p1);
        System.out.println("小李修改结果：" + result1);
        
        //4、小王将商品减了30元，存入了数据库
        p2.setPrice(p2.getPrice() - 30);
        int result2 = productMapper.updateById(p2);
        System.out.println("小王修改结果：" + result2);
        
        //最后的结果
        Product p3 = productMapper.selectById(1L);
        //价格覆盖，最后的结果：70
        System.out.println("最后的结果：" + p3.getPrice());
    }
```

## Mybatis-Plus实现乐观锁

### 修改实体类

> 给Version字段添加 `@Version` 注解

```java
@Data
public class Product {
    private Long id;
    private String name;
    private Integer price;
    @Version // 标识乐观锁版本号字段
    private Integer version;
}
```

### 添加乐观锁插件配置

> 和上面使用分页插件的时候一样的操作

```java
@Configuration
// 扫描Mapper接口所在的包
@MapperScan(value = {"com.along.mybatisplus.mapper"})
public class MybatisPlusConfig {
    // 添加一个MybatisPlus拦截器
    // 添加@Bean注解交给Spring容器管理
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        // 拿到拦截器
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 添加上page分页插件 PaginationInnerInterceptor
        // 给分页插件设置一个默认数据库类型 DbType.MYSQL（这里我用是Mysql）或者是DbType.SQL_SERVER都可以
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        // 添加乐观锁
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor()); // 这个才是乐观锁
        // 返回这个拦截
        return interceptor;
    }
}
```

> **现在再去测试代码和数据，最后得到的就是150了**

### 优化代码

```java
    @Test
    public void testProduct01() {
        //1、小李
        Product p1 = productMapper.selectById(1L);
        System.out.println("小李取出的价格：" + p1.getPrice());

        //2、小王
        Product p2 = productMapper.selectById(1L);
        System.out.println("小王取出的价格：" + p2.getPrice());

        //3、小李将价格加了50元，存入了数据库
        p1.setPrice(p1.getPrice() + 50);
        int result1 = productMapper.updateById(p1);
        System.out.println("小李修改结果：" + result1);

        //4、小王将商品减了30元，存入了数据库
        p2.setPrice(p2.getPrice() - 30);
        int result2 = productMapper.updateById(p2);
        System.out.println("小王修改结果：" + result2);

        if (result2 == 0) {
            // 操作失败，重试
            Product productNew = productMapper.selectById(1);
            productNew.setPrice(productNew.getPrice() - 30);
            productMapper.updateById(productNew);
        }
        // 最终得到结果120
    }
```

# 通用枚举

> 表中的有些字段值是固定的，例如性别（男或女），此时我们可以使用MyBatis-Plus的通用枚举来实现

## 数据库User表添加sex字段

![User表添加sex字段](Snipaste_2022-05-17_16-43-39.png)

## 创建通用枚举类型

> **给需要添加值数据库的枚举添加 `@EnumValue属性` **

```java
@Getter
public enum SexEnum {
    MALE(1, "男"),
    FEMALE(0, "女");
    
    @EnumValue // 将注解所标识的属性的值存到数据库中
    private Integer sex;
    private String sexName;

    SexEnum(Integer sex, String sexName) {
        this.sex = sex;
        this.sexName = sexName;
    }
}
```

## 配置扫描通用枚举

```yaml
mybatis-plus:
  # 扫描通用枚举的包
  type-enums-package: com.along.mybatisplus.enums
```

## 测试代码

> 如果这里报错，请检查枚举扫描器是否配置好
>
> 或者是application.yml中配置了主键策略为自增但是数据库没设置（如果是跟着本教程走的那八成就是了）

```java
    @Test
    public void test(){
        User user = new User();
        user.setName("廖狗");
        user.setAge(17);
        user.setSex(SexEnum.MALE);
        int result = userMapper.insert(user);
        System.out.println(result);
    }
```

# 代码生成器

## 引入Maven依赖

```xml
        <!-- 代码生成器核心依赖 -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.5.1</version>
        </dependency>
        <!-- freemarker引擎模板 -->
        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
            <version>2.3.31</version>
        </dependency>
```

## 快速生成

> 创建测试类运行代码就可以了

```java
public class FastAutoGeneratorTest {
    public static void main(String[] args) {
        FastAutoGenerator.create("jdbc:mysql://127.0.0.1:3306/mybatis_plus?characterEncoding=utf-8&userSSL=false", "root", "123456")
                .globalConfig(builder -> {
                    builder.author("along") // 设置作者
                            //.enableSwagger() // 开启 swagger 模式
                            .fileOverride() // 覆盖已生成文件
                            .outputDir("D://mybatis_plus"); // 指定输出目录
                })
                .packageConfig(builder -> {
                    builder.parent("com.along") // 设置父包名
                            .moduleName("mybatisplus") // 设置父包模块名
                            .pathInfo(Collections.singletonMap(OutputFile.mapperXml, "D://mybatis_plus"));
                                                                // 设置mapperXml生成路径
                })
                .strategyConfig(builder -> {
                    builder.addInclude("t_user") // 设置需要生成的表名
                            .addTablePrefix("t_", "c_"); // 设置过滤表前缀
                })
                .templateEngine(new FreemarkerTemplateEngine()) // 使用Freemarker 引擎模板，默认的是Velocity引擎模板
                .execute(); // 执行
    }
}
```

# 多数据源

> 适用于多种场景：纯粹多库、 读写分离、 一主多从、 混合模式等
>
> 目前我们就来模拟一个纯粹多库的一个场景，其他场景类似
> 场景说明：
>
> 我们创建两个库，分别为：mybatis_plus（以前的库不动）与mybatis_plus_1（新建），将mybatis_plus库的product表移动到mybatis_plus_1库，这样每个库一张表，通过一个测试用例分别获取用户数据与商品数据，如果获取到说明多库模拟成功

## 创建数据库表

> 创建数据库mybatis_plus_1和表product

```mysql
CREATE DATABASE `mybatis_plus_1` /*!40100 DEFAULT CHARACTER SET utf8mb4 */;
use `mybatis_plus_1`;
CREATE TABLE product
(
    id BIGINT(20) NOT NULL COMMENT '主键ID',
    name VARCHAR(30) NULL DEFAULT NULL COMMENT '商品名称',
    price INT(11) DEFAULT 0 COMMENT '价格',
    version INT(11) DEFAULT 0 COMMENT '乐观锁版本号',
    PRIMARY KEY (id)
);

```

> 添加测试数据

```mysql
INSERT INTO product (id, NAME, price) VALUES (1, '外星人笔记本', 100);
```

> 删除mybatis_plus库product表 （如果没有这个数据库请看目录初始化）

```mysql
use mybatis_plus;
DROP TABLE IF EXISTS product;
```

## 引入依赖

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>dynamic-datasource-spring-boot-starter</artifactId>
    <version>3.5.0</version>
</dependency>
```

## 配置多数据源

> 说明：注释掉之前的数据库连接，添加新配置

```yaml
spring:
  # 配置数据源信息
  datasource:
    dynamic:
      # 设置默认的数据源或者数据源组,默认值即为master
      primary: master
      # 严格匹配数据源,默认false.true未匹配到指定数据源时抛异常,false使用默认数据源
      strict: false
      datasource:
        master:
          url: jdbc:mysql://localhost:3306/mybatis_plus?characterEncoding=utf-8&useSSL=false
          driver-class-name: com.mysql.cj.jdbc.Driver
          username: root
          password: 123456
        slave_1:
          url: jdbc:mysql://localhost:3306/mybatis_plus_1?characterEncoding=utf-8&useSSL=false
          driver-class-name: com.mysql.cj.jdbc.Driver
          username: root
          password: 123456
```

## 创建Mapper

> PrudoctMapper.java

```java
@Repository
@DS("slave_1") // 指定数据库
public interface ProductMapper extends BaseMapper<Product> {}
```

> UserMapper.java

```java
@Repository
public interface UserMapper extends BaseMapper<User> {}
```

## 创建Service

> 这里演示Product，自己动手操作,不会的话看[这里](#创建Service以及实现类)，一样的方法······

```java
public interface ProductService extends IService<Product> {}
```

```java
@Service
public class ProductMapperImpl extends ServiceImpl<ProductMapper, Product> implements ProductService {}
```



##  测试

```java
    @Test
    public void test(){
        System.out.println(productService.getById(1));
        System.out.println(userService.getById(1));
    }
```

> 结果：
>
> Product(id=1, name=外星人笔记本, price=100, version=0)
> User(id=1, name=Jone, age=18, sex=null, email=test1@baomidou.com, isDeleted=0)

# 使用IDEA MybatisX插件生成代码

> MyBatis-Plus为我们提供了强大的mapper和service模板，能够大大的提高开发效率但是在真正开发过程中，MyBatis-Plus并不能为我们解决所有问题，例如一些复杂的SQL，多表联查，我们就需要自己去编写代和SQL语句，我们该如何快速的解决这个问题呢，这个时候可以使用MyBatisX插件。
>
> MyBatisX一款基于 IDEA 的快速开发插件，为效率而生。

![IDEA下载MybatisX插件](Snipaste_2022-05-17_21-19-55.png)

![IDEA连接数据库](Snipaste_2022-05-17_21-20-55.png)

![代码生成设置](Snipaste_2022-05-17_21-23-23.png)

![代码生成设置2](Snipaste_2022-05-17_21-24-01.png)

**MyBatisX插件用法：https://baomidou.com/pages/ba5b24**

