# IDEA插件 Easycode研究

## 1. Easy code插件研究

Easycode是idea的一个插件，可以直接对数据的表生成entity,controller,service,dao,mapper,无需任何编码，简单而强大。

1、安装(EasyCode)

![EASY Code](https://image.uczzd.cn/1659652693635114170.jpg?id=0)

我这里的话是已经那装好了。

建议大家在安装一个插件，叫做Lombok。Lombok能通过注解的方式，在编译时自动为属性生成构造器、getter/setter、equals、hashcode、toString方法。出现的神奇就是在源码中没有getter和setter方法，但是在编译生成的字节码文件中有getter和setter方法。

2、建立数据库

```
-- ------------------------------ Table structure for user-- ----------------------------DROP TABLE IF EXISTS `user`;CREATE TABLE `user` (

  `id` int(11) NOT NULL,

  `username` varchar(20) DEFAULTNULL,

  `sex` varchar(6) DEFAULTNULL,

  `birthday` date DEFAULTNULL,

  `address` varchar(20) DEFAULTNULL,

  `password` varchar(20) DEFAULTNULL,

  PRIMARY KEY (`id`)) ENGINE=InnoDB DEFAULT CHARSET=utf8;SET FOREIGN_KEY_CHECKS = 1;
```

3、在IDEA配置连接数据库

在这个之前，新建一个Springboot项目，这个应该是比较简单的。

建好SpringBoot项目之后，如下图所示，找到这个Database

![EasyCode](https://image.uczzd.cn/464717806018931146.jpg?id=0&width=720)

按照如下图所示进行操作：

![EasyCode](https://image.uczzd.cn/11051346294967259041.jpg?id=0&width=720)

然后填写数据库名字，用户名，密码。点击OK即可。这样的话，IDEA连接数据库就完事了。

![EasyCode](https://image.uczzd.cn/4471730701661600214.jpg?id=0&width=720)

4、开始生成代码

在这个里面找到你想生成的表，然后右键，就会出现如下所示的截面。

![EasyCode](https://image.uczzd.cn/1963802877390081264.jpg?id=0&width=720)

点击1所示的位置，选择你要将生成的代码放入哪个文件夹中，选择完以后点击OK即可。

![EasyCode](https://image.uczzd.cn/7205175409586892090.jpg?id=0&width=720)

勾选你需要生成的代码，点击OK。

![EasyCode](https://image.uczzd.cn/49236926242065929.jpg?id=0&width=720)

这样的话就完成了代码的生成了，生成的代码如下图所示：

![EasyCode](https://image.uczzd.cn/4275485132955620882.jpg?id=0&width=720)

5、pom.xml

```
org.springframework.boot

spring-boot-starter


org.springframework.boot

spring-boot-starter-web


org.projectlombok

lombok

true


org.springframework.boot

spring-boot-devtools

true


org.mybatis.spring.boot

mybatis-spring-boot-starter

1.3.2


mysql

mysql-connector-java

5.1.47


com.alibaba

druid

1.0.9
```

6、Application.yml

```
server:

port: 8089

spring:

datasource:

url: jdbc:mysql://127.0.0.1:3306/database?useUnicode=true&characterEncoding=UTF-8

username: root

password: 123456

type: com.alibaba.druid.pool.DruidDataSource

driver-class-name: com.mysql.jdbc.Driver

mybatis:

mapper-locations: classpath:/mapper/*Dao.xml

typeAliasesPackage: com.vue.demo.entity
```

7、启动项目

在启动项目之前，我们需要先修改两个地方。

在dao层加上@mapper注解

![EasyCode](https://image.uczzd.cn/10223961605881033454.jpg?id=0&width=720)

在启动类里面加上@MapperScan("com.vue.demo.dao")注解。

![EasyCode](https://image.uczzd.cn/18330394874570264439.jpg?id=0&width=720)

启动项目

![EasyCode](https://image.uczzd.cn/18356017071014140193.jpg?id=0&width=720)
测试一下

![EasyCode](https://image.uczzd.cn/519263529953969417.jpg?id=0&width=720)

![EasyCode](https://image.uczzd.cn/14039073288447076677.jpg?id=0&width=720)

## 2. IDEA进阶应用

来源：
https://www.toutiao.com/a6632456747728503304/

另外一篇使用教程：

https://www.toutiao.com/a6673708045421249027/


