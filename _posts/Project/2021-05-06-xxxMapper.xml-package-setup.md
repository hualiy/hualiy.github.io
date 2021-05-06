---
layout: post
title: xxxMapper.xml 放在java包下打包被忽略  
date: 2021-05-06
categories: [project]
tags: [xml]
excerpt: project configuration
---



**xxxMapper.xml放在src/main/java 下不会被打包,pom.xml中配置一下**

```xml
<build>
        <!--  xml写在包下面默认会过滤掉,配置一下.将包下的xml打包      -->
        <resources>
            <resource>
                <directory>src/main/resources</directory>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
        </resources>
</build>
```

