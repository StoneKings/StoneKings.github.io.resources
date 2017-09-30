---
title: MyBatis获取插入数据主键
date: 2017-06-19 13:52:32
tags: Java EE学习
---

## ibatis获取主键方式
在ibatis时代我们通过  SELECT LAST_INSERT_ID()方式获取主键
DAO接口内方法声明
```java
//返回的值为新插入数据的ID
Long insert(Student student);
```
Mapper文件配置
```xml
<insert id="insert" parameterClass="Student">
        insert into student
          (name, class, grade)
        values
          (#name#, #class#, #grade#);
        <selectKey  keyProperty="id" resultClass="java.lang.Long">
            SELECT LAST_INSERT_ID()
        </selectKey>
    </insert>
```

使用方法
```java
Student student = new Student("张三","1","3")//初始化student的name，class，grade
Long id = studentDao.insert(student);   //获取插入ID
```

## Mybatis 获取主键方式
DAO接口方法声明
```java
//这里返回值仅仅表示受影响的数据条数，如果插入成功返回1，如果插入失败返回0
int insert(Student student);
```
Mapper文件配置
```xml
<insert id="insert" parameterType="Student">
        insert into student
          (name, class, grade)
        values
          (#{name}, #{class}, #{grade});
</insert>
```

使用方法
```java
Student student = new Student("张三","1","3")//初始化student的name，class，grade
int count = studentDao.insert(student);   //获取插入ID
if(count == 1){
  Long id = student.getId();  //得到主键
}else{
  System.out.println("插入数据错误");
}
```
没错，这里插入后会直接返回含有插入ID的student，和ibatis的差别还是有点大的

