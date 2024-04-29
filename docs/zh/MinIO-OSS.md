## MinIO OSS服务器

MinIO OSS服务器是一种高性能、分布式的对象存储服务器，基于Apache License v2.0开源协议，与Amazon S3兼容。它使用MinIO客户端库、MinIO Gateway或其他S3兼容的客户端进行访问，适用于存储大量非结构化数据，如图片、视频、日志文件等。

MinIO OSS服务器具有以下特点：

-   **高性能**：MinIO OSS服务器采用高性能的分布式架构设计，可以处理大量并发读写请求，提供高吞吐量的数据存储服务。
-   **可扩展性**：MinIO OSS服务器支持在多个节点上进行横向扩展，可以轻松地增加存储容量和性能。
-   **安全性**：MinIO OSS服务器支持访问控制列表（ACL）、加密存储、数据完整性校验等安全功能，确保数据的安全性和完整性。
-   **兼容性**：MinIO OSS服务器与Amazon S3兼容，可以使用S3兼容的客户端库和工具进行访问和管理。

MinIO OSS服务器是一种高性能、可扩展、安全可靠的分布式对象存储解决方案，适用于各种需要存储大量非结构化数据的场景

### 快速开始

```properties
# 服务器地址
url:192.168.74.72
port:9000
Access Key:0r0cM7crAokUFdwr5AU7
Secret Key:oFSJgk3pEttWUCoFWiAh4WK1usGa5eYWeT8hETgN
```

#### 导入依赖

**首先，我们需要导入maven依赖**

```java
<!-- https://mvnrepository.com/artifact/io.minio/minio -->
<dependency>
    <groupId>io.minio</groupId>
    <artifactId>minio</artifactId>
    <version>8.5.9</version>
</dependency>
```

#### 上传代码示例

**我们需要一个minioClient对象，设置url，port，ssl（false）**

```java
MinioClient minioClient = MinioClient.builder()
                .endpoint("192.168.74.72", 9000, false)		// 设置url，端口，SSL  设置密钥↓
                .credentials("0r0cM7crAokUFdwr5AU7", "oFSJgk3pEttWUCoFWiAh4WK1usGa5eYWeT8hETgN")
                .build();
```

**接下来我们开始使用这个对象进行上传**

```java
boolean found =
        minioClient.bucketExists(BucketExistsArgs.builder().bucket("java-test").build());
if (!found) {	// 如果找不到此储存池，则创建，这里可以把储存池换成自己的唯一标识，在这里java-test为自定义储存池
    minioClient.makeBucket(MakeBucketArgs.builder().bucket("java-test").build());
} else {
    System.out.println("Bucket 'java-test' already exists.");
}
// 以上为校验储存池是否存在，只需要运行一次，下面是上传文件逻辑
// 我们即将将本地文件C:\\Users\\TK.ENDO\\Pictures\\微信图片_20240326100209.jpg
// 上传至远程服务器的wechat_20240326100209.jpg
minioClient.uploadObject(	// 使用uploadObject方法
        UploadObjectArgs.builder()
                .bucket("java-test")	// 选择储存池，可以
                .object("wechat_20240326100209.jpg")	// 选择保存在服务器的文件名称
                .filename("C:\\Users\\TK.ENDO\\Pictures\\微信图片_20240326100209.jpg")	// 选择本地文件路径
                .build());	// 执行构建操作
System.out.println("保存成功！");
```

**至此，我们就成功上传了一个文件到远程服务器**

#### 获取url示例

```java
String url = minioClient.getPresignedObjectUrl(		// 使用getPresignedObjectUrl方法获取文件url
                    GetPresignedObjectUrlArgs.builder()
                            .method(Method.GET)		// 使用GET方法获取url值
                            .bucket("java-test")	// 选择文件所在储存池
                            .object("wechat_20240326100209.jpg")	// 选择文件名字
                            .expiry(1, TimeUnit.DAYS)
                            .build());
System.out.println(url);	// 打印url
```

**根据返回的url可以直接在浏览器打开图片**

## 进阶用法

每次上传文件的时候都需要重新创建对象，这样会造成资源的浪费，由于我们使用springboot框架，我们可以使用springboot框架自带的IOC容器进行控制反转，使用自动依赖注入（自动装配）来进行minioClient对象的管理，由于我们的minioClient是服务层

**在application.properties文件中定义连接配置**

```properties
# [MinIOConfig]
minio.configuration.end-point=192.168.74.72
minio.configuration.port=9000
minio.configuration.secure=false
minio.configuration.access-key=0r0cM7crAokUFdwr5AU7
minio.configuration.secret-key=oFSJgk3pEttWUCoFWiAh4WK1usGa5eYWeT8hETgN
```

我们定义了所需的url和端口等配置

**在service文件夹下创建MinioClientBuilder工具类**

```java
package com.rcilabarary.springbootwebtalisgroup2demo.service.impl;

import io.minio.MinioClient;
import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Primary;
import org.springframework.stereotype.Service;

@ConfigurationProperties(prefix = "minio.configuration")	// 配置批量注入
@Data		// 生成get set方法，重要
@Service
public class MinioClientBuilder {
    String endPoint;
    int port;
    boolean secure;
    String accessKey;
    String secretKey;

    @Bean
    @Primary
    public MinioClient minioClient() {
        return MinioClient.builder()
                .endpoint(endPoint, port, secure)        // 设置url，端口，SSL  设置密钥↓
                .credentials(accessKey, secretKey)
                .build();
    }

}   // Class end.
```

**由于我们在工具类中可以很方便的构建出MinioClient对象，我们还使用了@Service注解声明了Bean，框架会自动注入**

**在测试中新建测试文件MinIOTest.java**

```java
package com.rcilabarary.springbootwebtalisgroup2demo;

import io.minio.*;
import io.minio.errors.*;
import io.minio.http.Method;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.io.IOException;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.util.concurrent.TimeUnit;

@SpringBootTest
public class MinIOTest {
    @Autowired
    private MinioClient minioClient;		// 依赖自动装配
    @Test
    void testMinIO() throws IOException, ServerException, InsufficientDataException, ErrorResponseException, NoSuchAlgorithmException, InvalidKeyException, InvalidResponseException, XmlParserException, InternalException {
        boolean found = minioClient.bucketExists(
                        BucketExistsArgs.builder()
                                .bucket("java-test")
                                .build());
        if (!found) {		// 判断储存池是否存在，如不存在，则创建
            minioClient.makeBucket(MakeBucketArgs.builder().bucket("java-test").build());
        } else {
            System.out.println("Bucket 'java-test' already exists.");
        }
		// 文件上传逻辑...
        // 文件url获取逻辑...
        // 下面单独说明
    }

}   // Class end.

```

**文件上传逻辑**

```java
minioClient.uploadObject(
        UploadObjectArgs.builder()
                .bucket("java-test")
                .object("wechat_2024.jpg")		// 远程服务器文件名可以改
                .filename("C:\\Users\\TK.ENDO\\Pictures\\微信图片_20240326100209.jpg")
                .build());
System.out.println(" 保存了 " + "object 'wechat_20240326100209.jpg' to bucket 'java-test'.");
```

**获取url逻辑**

```java
String url = minioClient.getPresignedObjectUrl(
        GetPresignedObjectUrlArgs.builder()
                .method(Method.GET)
                .bucket("java-test")
                .object("wechat_2024.jpg")
                .expiry(1, TimeUnit.DAYS)
                .build());
System.out.println(url);
```

一切就绪之后，执行程序，你可能会获得这样的结果，点击连接则可以查看Minio OSS服务器上的远程图片

```java
Bucket 'java-test' already exists.
 保存了 object 'wechat_20240326100209.jpg' to bucket 'java-test'.
http://192.168.74.72:9000/java-test/wechat_2024.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=0r0cM7crAokUFdwr5AU7%2F20240419%	2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240419T082347Z&X-Amz-Expires=86400&X-Amz-SignedHeaders=host&X-Amz-Signature=09ad571a1545a8c791838767c0e9494acb389e2b3a00a4b6f96dccff0bb8bddc

进程已结束，退出代码为 0
```