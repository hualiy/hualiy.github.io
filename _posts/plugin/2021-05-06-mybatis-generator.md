---
layout: post
title: mybatis generator 代码生成器
date: 2021-05-06
categories: [plugin]
tags: [mybatisgenerator]
excerpt: mybatis generator 逆向工程
---

**pom.xml文件添加依赖和插件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cn.hwali</groupId>
    <artifactId>common-reverse</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <!-- 依赖 MyBatis 核心包 -->
    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.2.8</version>
        </dependency>
    </dependencies>
    
    <!-- 控制 Maven 在构建过程中相关配置 -->
    <build>
        <!-- 构建过程中用到的插件 -->
        <plugins>
            <!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.0</version>
                <!-- 插件的依赖 -->
                <dependencies>
                    <!-- 逆向工程的核心依赖 -->
                    <dependency>
                        <groupId>org.mybatis.generator</groupId>
                        <artifactId>mybatis-generator-core</artifactId>
                        <version>1.3.2</version>
                    </dependency>
                    <!-- 数据库连接池-->
                    <dependency>
                        <groupId>com.alibaba</groupId>
                        <artifactId>druid</artifactId>
                        <version>1.1.10</version>
                    </dependency>
                    <!--MySQL 驱动 -->
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>5.1.8</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>
</project>
```

<br/>

**generatorConfig.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- （1）配置mysql 驱动jar包路径.用了绝对路径 -->
       <!-- <classPathEntry location=
                                "G:\0-maven\repository-BD\repository\mysql\mysql-connector-java\5.1.38\mysql-connector-java-5.1.38.jar" />-->

    <context id="hwali_mysql_tables" targetRuntime="MyBatis3">
        <!-- 防止生成的代码中有很多注释，加入下面的配置控制 -->
        <commentGenerator>
            <property name="suppressAllComments" value="true"/>
            <!-- <property name="suppressDate" value="true" />-->
        </commentGenerator>

        <!-- （2）数据库连接 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/demo"
                        userId="hwali"
                        password="hwali">
        </jdbcConnection>

        <!-- 默认 false，把 JDBCDECIMAL 和 NUMERIC 类型解析为 Integer，
        为 true 时把 JDBCDECIMAL 和 NUMERIC 类型解析为 java.math.BigDecimal-->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!-- （3）数据表对应的model层  targetProject:生成 Entity 类的路径 -->
        <javaModelGenerator targetPackage="cn.hwali.model"
                            targetProject="src\main\java">
            <!--enableSubPackages:是否让 schema 作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- (4)sql mapper 映射配置文件 -->
        <sqlMapGenerator targetPackage="cn.hwali.mapper"
                         targetProject="src\main\java">
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>

        <!-- (5)mybatis3中的mapper接口 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="cn.hwali.mapper"
                             targetProject="src\main\java">
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>
        
        <!-- 不生成example类 -->
        <table tableName="xx" 
               domainObjectName="Xx" 
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false">
        </table>
    </context>
</generatorConfiguration>

```

