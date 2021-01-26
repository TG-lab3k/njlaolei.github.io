---
layout: post
title: "Spring IoC"
categories: Spring
---
## 1 核心类
BeanFactory
ApplicationContext

#### Bean
1. 构建
A. 通过构造函数构建
   <bean class="">
B. 通过类的static方法构建
   <bean class="", factory-method="">
C. 通过一个指定的Factory实例构建
  <bean factory-bean="", factory-method="">

2. 依赖
A. 依赖注入(Dependency Injection)
   1). 通过构造函数注入依赖
   ```
   <bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
   </bean>
   ```
   2). 通过setter方法注入依赖
   ```
   <bean id="exampleBean" class="examples.ExampleBean">
    <!-- setter injection using the nested ref element -->
    <property name="beanOne">
        <ref bean="anotherExampleBean"/>
    </property>

    <!-- setter injection using the neater ref attribute -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
   </bean>

   <bean id="anotherExampleBean" class="examples.AnotherBean"/>
   <bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
   ```
   
