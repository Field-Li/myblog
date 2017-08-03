---
layout: post
title: Mybatis-Generator
categories: [Tech]
---

{{ page.title }}
================

<p class="meta">3 August 2017 - ShangHai</p>

记录一些提高工作效率的小技巧。
Mybatis-Generator自动生成一些DAO的代码，表对应的实体、各种sql语句、example类等。


一、在resource目录存放Mybatis-generator.xml：

{% highlight java %}
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration PUBLIC
        "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd" >
<generatorConfiguration>

    <!-- !!!! Driver Class Path !!!! -->
    <classPathEntry location="D:\Users\  a\.m2\repository\mysql\mysql-connector-java\5.1.35\mysql-connector-java-5.1.35.jar"/>

    <context id="context" targetRuntime="MyBatis3">
        <commentGenerator>
            <property name="suppressAllComments" value="false"/>
            <property name="suppressDate" value="true"/>
        </commentGenerator>

        <!-- !!!! Database Configurations !!!! -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://groupwormhole.mysql.db.fat.qa.nt. corp.com:55111/groupwormholedb?characterEncoding=utf8"
                        userId="us_test_  a"
                        password="rootroot"/>

        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!-- !!!! Model Configurations !!!! -->
        <javaModelGenerator targetPackage="com.henryxi.mybatis.entity" targetProject="D:\code\lodge\ProductJobWs\httpweb\src\main">
            <property name="enableSubPackages" value="false"/>
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- !!!! Mapper XML Configurations !!!! -->
        <sqlMapGenerator targetPackage="com.henryxi.mybatis.entity" targetProject="D:\code\lodge\ProductJobWs\httpweb\src\main">
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>

        <!-- !!!! Mapper Interface Configurations !!!! -->
        <javaClientGenerator targetPackage="com.henryxi.mybatis.mapper" targetProject="D:\code\lodge\ProductJobWs\httpweb\src\main" type="XMLMAPPER">
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>

        <!-- !!!! Table Configurations !!!! -->
        <table tableName="bnb_space" domainObjectName="BnbSpace"  enableCountByExample="true" enableDeleteByExample="true" enableSelectByExample="true"
               enableUpdateByExample="true"/>
    </context>
</generatorConfiguration>
{% endhighlight %}

二、右击该文件，运行就可以了。（这种方式运行需提前建立好package）
![My helpful screenshot]({{ site.url }}/assets/techPic/mybatis-generator.PNG)

也可以通过Mybatis 插件来运行。在pom.xml中加入如下plugin：
{% highlight java %}
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.2</version>
    <configuration>
        <configurationFile>src/main/resources/mybatis-generator.xml</configurationFile>
        <verbose>true</verbose>
        <overwrite>true</overwrite>
    </configuration>
    <executions>
        <execution>
            <id>Generate MyBatis Artifacts</id>
            <goals>
                <goal>generate</goal>
            </goals>
        </execution>
    </executions>
    <dependencies>
        <dependency>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-core</artifactId>
            <version>1.3.2</version>
        </dependency>
    </dependencies>
</plugin>
{% endhighlight %}
![My helpful screenshot]({{ site.url }}/assets/techPic/mybatis-generator2.PNG)
