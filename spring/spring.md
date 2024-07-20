# spring核心概念

![image-20240716104233567](D:/Blog/source/_posts/杂题结论/image-20240716104233567.png)

![image-20240716104628943](D:/Blog/source/_posts/杂题结论/image-20240716104628943.png)

## 1.1IOC概述

IOC:控制反转（inversion of control）

easy know:对象的创建是new，改为spring框架完成，底层是利用了反射。

一般：

```java
User user = new User();
user.setId(101);
user.setName("zs");
user.setPassword("123");
"a.b.c.entity.User"
Class userClass =Class.forName("a.b.c.entity.User");
User user=(User)userClass.newInstance();//字节码
idMethod=userClass...
idMethod.call(user,101)...
```

![img](https://img-blog.csdn.net/20170513133210763)

## 1.2 反射实现对象注入与展示

反射实现

```java
import org.junit.Test;

import java.lang.reflect.Method;

public class Main {
    @Test
    public static void main(String[] args) {
        Class<?> objClass=Class.forName("com.example.MyClass");
        Object obj = objClass.newInstance();
        Method setidMethod=obj.getMethod("set"+"User_id",Integer.class);
        Method setaccountMethod=obj.getMethod("set"+"User_account",Integer.class);
        Method setpasswordMethod=obj.getMethod("set"+"User_password",Integer.class);
        setidMethod.invoke(obj,101);
        setaccountMethod.invoke(obj,"zs");
        setpasswordMethod.invoke(obj,123);
    }
}
```



## 1.3IOC入门案例

spring框架怎么做



## 1.4IOC和DI

DI：依赖注入（Dependence Injection）也是反射的一环。

## 1.5AOP概述

面向切面编程（Aspect-Oriented Programing)

早期写事务

```java
try{
zs账户减少
ls账户增加
扣除
...
commit
}catch(ex){
rollback回滚
}finally{
close释放
}
//像这样的很多很多个
```

![image-20240716112508077](D:/Blog/source/_posts/杂题结论/image-20240716112508077.png)

## 1.6业务类和事务管理类配置

![image-20240716112839149](D:/Blog/source/_posts/杂题结论/image-20240716112839149.png)

配置到spring

## 1.8AOP配置

配置到xml

## 1.9容器中userservice增强

## 1.10七个术语

1.Advice通知：增强别人类，事务管理类

2.PointCut切入点：可以被切入的方法，业务类中的方法

3.JoinPiont连接点：已经被切入的方法

4.Aspect切面：将通知和连接点整合，形成那个面。增强代理类中的被增强方法

5.Target object目标对象：被增强的类，业务类

6.AOP proxy AOP代理：目标对象和通知，交织在一起形成新类

7.Weaving织入：通知+目标对象整合的过程

## 1.11构造注入



## 1.12原型

## 1.13复杂数据类型注入1

## 1.14复杂数据类型注入2

## 1.15学习注释前准备工作

## 1.16注解方式实现IOC

## 1.17注解方式实现AOP

![image-20240716114445906](D:/Blog/source/_posts/杂题结论/image-20240716114445906.png)
