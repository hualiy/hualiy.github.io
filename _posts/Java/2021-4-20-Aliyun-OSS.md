---
layout: post
title: Aliyun OSS
date: 2021-05-19
categories: [Java,UpLoad]
tags: [OSS,Java,Upload]
excerpt: Aliyun OSS
---

pom.xml

```xml
 <!-- https://mvnrepository.com/artifact/com.aliyun.oss/aliyun-sdk-oss -->
        <dependency>
            <groupId>com.aliyun.oss</groupId>
            <artifactId>aliyun-sdk-oss</artifactId>
            <version>3.9.1</version>
        </dependency>
```



application.yml

```yaml
aliyun:
  oss:
    access-key-id: 
    access-key-secret: 
    bucket-domain: 
    bucket-name: hual
    end-point: 
```



OSSProperites.class

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Component
@ConfigurationProperties(prefix = "aliyun.oss")
public class OSSProperties {
    private String endPoint;
    private String bucketName;
    private String accessKeyId;
    private String accessKeySecret;
    private String bucketDomain;
}
```



```java
public static RespBean uploadFileToOss(
            String endpoint,
            String accessKeyId,
            String accessKeySecret,
            String bucketName,
            String bucketDomain,
            InputStream inputStream,
            String originalName
    ){

        //创建OSSClient实例
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        //生成上传文件的目录
        String folderName = new SimpleDateFormat("yyyyMMdd").format(new Date());

        // 生成上传文件在OSS服务器上保存时的文件名
        // 原始文件名：beautfulgirl.jpg
        // 生成文件名：wer234234efwer235346457dfswet346235.jpg
        // 使用UUID生成文件主体名称
        String fileMainName = UUID.randomUUID().toString().replace("-", "");

        // 从原始文件名中获取文件扩展名
        String extensionName = originalName.substring(originalName.lastIndexOf("."));

        // 使用目录、文件主体名称、文件扩展名称拼接得到对象名称
        String objectName = folderName + "/" + fileMainName + extensionName;

        try {
            // 调用OSS客户端对象的方法上传文件并获取响应结果数据
            PutObjectResult putObjectResult = ossClient.putObject(bucketName, objectName, inputStream);

            // 从响应结果中获取具体响应消息
            ResponseMessage responseMessage = putObjectResult.getResponse();

            // 根据响应状态码判断请求是否成功
            if(responseMessage == null) {

                // 拼接访问刚刚上传的文件的路径
                String ossFileAccessPath = bucketDomain + "/" + objectName;

                // 当前方法返回成功
                return RespBean.ok("更新成功！",ossFileAccessPath);
            } else {
                // 获取响应状态码
                int statusCode = responseMessage.getStatusCode();

                // 如果请求没有成功，获取错误消息
                String errorMessage = responseMessage.getErrorResponseAsString();

                // 当前方法返回失败
                return RespBean.error("当前响应状态码="+statusCode+" 错误消息="+errorMessage);
            }
        } catch (Exception e) {
            e.printStackTrace();

            // 当前方法返回失败
            return RespBean.error(e.getMessage());
        } finally {

            if(ossClient != null) {

                // 关闭OSSClient。
                ossClient.shutdown();
            }
        }
    }
```

