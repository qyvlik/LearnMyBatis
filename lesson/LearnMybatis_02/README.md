# 使用MyBatis对表执行CRUD操作

> 数据库初始化参照 [MyBatis 入门](../LearnMyBatis_01/README.md)

> [使用MyBatis对表执行CRUD操作](http://www.linuxidc.com/Linux/2015-02/113771p2.htm)

## 配置文件

### 添加

添加用户，对应的数据库操作时插入：

```
int result = SqlSession.insert("PackageName.MapperXML.InsertMethod", Entity);
SqlSession.close();
```

### 更新

```
int result = SqlSession.update("PackageName.MapperXML.UpdateMethod", Entity);
SqlSession.close();
```

### 删除

```
int result = SqlSession.delete("PackageName.MapperXML.DeleteMethod", EntityID);
SqlSession.close();
```

### 获取所有记录

> SqlSession.SelectList 有三个重载函数，可以传入参数，进行分页，后续会讲到~

```
List<Entity> entitys = SqlSession.SelectList("PackageName.MapperXML.DeleteMethod");
SqlSession.close();
```

## 注解

不同于配置文件。在执行 sql 操作时，是通过 SqlSession.getMapper 获取到一个被注入方法的接口对象。然后操作这个接口对象执行 sql 操作。
