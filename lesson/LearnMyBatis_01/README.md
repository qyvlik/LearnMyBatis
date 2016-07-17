# Learn MyBatis 入门

> [MyBatis入门学习教程](http://www.linuxidc.com/Linux/2015-02/113771.htm)

> 用 IntelliJ IDEA 14.0.3 打开~

## 加入 jar 文件

## 初始化数据库

注意编码和排序规则。

> ci是 case insensitive, 即 "大小写不敏感", a 和 A 会在字符判断中会被当做一样的;bin 是二进制, a 和 A 会别区别对待.例如你运行:SELECT * FROM table WHERE txt = 'a'那么在utf8_bin中你就找不到 txt = 'A' 的那一行, 而 utf8_general_ci 则可以.

```
create database mybatis;
use mybatis;
CREATE TABLE users(id INT PRIMARY KEY AUTO_INCREMENT, NAME VARCHAR(20), age INT);
INSERT INTO users(NAME, age) VALUES('孤傲苍狼', 27);
INSERT INTO users(NAME, age) VALUES('白虎神皇', 27);
```

## 编写配置文件

在 `src` 目录下新建一个名为 `config.xml` 的文件。这个配置文件是保存数据库配置的。注意修改为自己的数据库用户名和密码。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <!-- 配置数据库连接信息 -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

## 添加表对应的实体类

`org.qyvlik.mybatis.entity.User`

```
public class User {
    //实体类的属性和表的字段名称一一对应
    private int id;
    private String name;
    private int age;

    // ...
}
```

## 定义操作 users 表的 sql 映射文件 userMapper.xml

为这个 `mapper` 指定一个唯一的 `namespace`，`namespace` 的值习惯上设置成包名+ `sql` 映射文件名，这样就能够保证 `namespace` 的值是唯一的。

例如 `namespace="org.qyvlik.mybatis.mapping.userMapper"` 就是 `org.qyvlik.mybatis.mapping.userMapper` (包名)+ `userMapper` (userMapper.xml文件去除后缀)。

在 `select` 标签中编写查询的 `SQL` 语句， 设置 `select` 标签的id属性为 `getUser`，`id` 属性值必须是唯一的，不能够重复使用 `parameterType` 属性指明查询时使用的参数类型，`resultType` 属性指明查询返回的结果集类型 `resultType="qyvlik.mybatis.entity.User"` 就表示将查询结果封装成一个 `User` 类的对象返回 `User` 类就是 `users` 表所对应的实体类


```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.qyvlik.mybatis.mapping.userMapper">

    <!--
         根据id查询得到一个user对象
       -->
    <select id="getUser" parameterType="int"
            resultType="org.qyvlik.mybatis.entity.User">
        select * from users where id=#{id}
    </select>
</mapper>
```

## 在 `conf.xml` 文件中注册 `userMapper.xml` 文件

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <!-- 配置数据库连接信息 -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!-- 注册userMapper.xml文件-->
        <mapper resource="org/qyvlik/mybatis/mapping/userMapper.xml"/>
    </mappers>
</configuration>
```

## 测试代码

见 [Test1](src/org.qyvlik.mybatis.test.Test1.java)。

遇到问题：

>  Mapped Statements collection does not contain value for

解决：注意 `userMapper.xml` 中 `namespace` 标签的包名设定。
