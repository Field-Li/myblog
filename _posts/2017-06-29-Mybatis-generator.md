---
layout: post
title: Mybatis-Generator工具使用
categories: [Tech]
---

{{ page.title }}
================

<p class="meta">29 June 2017 - ShangHai</p>


{% highlight java %}
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration PUBLIC
 "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
 "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd" >
<generatorConfiguration>
 
    <!-- !!!! Driver Class Path !!!! -->
 <classPathEntry location="D:\Users\.m2\repository\mysql\mysql-connector-java\5.1.35\mysql-connector-java-5.1.35.jar"/>
 
    <context id="context" targetRuntime="MyBatis3">
        <commentGenerator>
            <property name="suppressAllComments" value="false"/>
            <property name="suppressDate" value="true"/>
        </commentGenerator>
 
        <!-- !!!! Database Configurations !!!! -->
 <jdbcConnection driverClass="com.mysql.jdbc.Driver"
 connectionURL="jdbc:mysql://groupwormholedb?characterEncoding=utf8"
 userId="xxx"
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
 <table tableName="bnb_space" domainObjectName="BnbSpace" enableCountByExample="true" enableDeleteByExample="true" enableSelectByExample="true"
 enableUpdateByExample="true"/>
    </context>
</generatorConfiguration>
{% endhighlight %}
