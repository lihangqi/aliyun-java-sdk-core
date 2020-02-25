<h1 align="center">Aliyun Core SDK for Java</h1> 

[![Build Status](https://travis-ci.com/lihangqi/aliyun-java-sdk-core.svg?branch=master)](https://travis-ci.com/lihangqi/aliyun-java-sdk-core)
[![Coverage Status](https://coveralls.io/repos/github/lihangqi/aliyun-java-sdk-core/badge.svg?branch=master)](https://coveralls.io/github/lihangqi/aliyun-java-sdk-core?branch=master)

SDK核心库

Maven依赖：[Maven Repository](https://mvnrepository.com/artifact/com.aliyun/aliyun-java-sdk-core)

当使用Alibaba Cloud SDK for Java访问阿里云服务时，您需要提供阿里云账号进行身份验证。
目前，Alibaba Cloud SDK for Java支持以下几种身份验证方式：

验证方式|说明
|------|----
AccessKey|使用AccessKey ID和AccessKey Secret访问
StsToken|使用STS Token访问
RamRoleArn|使用RAM子账号的AssumeRole方式访问
EcsRamRole|在ECS实例上通过EcsRamRole实现免密验证
RsaKeyPair|使用RSA公私钥方式（仅日本站支持）

以AccessKey为例说明如何设置身份凭证。为了保证您的账号安全，建议您使用RAM账号来访问阿里云服务。阿里云账号的AccessKey对拥有的资源有完全的权限。RAM账号由阿里云账号授权创建，仅有对特定资源限定的操作权限。

用AccessKey作为访问凭据，需要在初始化Client时设置凭证。示例代码如下：
```java
package com.testprogram;
import com.aliyuncs.profile.DefaultProfile;
import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.ecs.model.v20140526.*;
public class Main {
    public static void main(String[] args) {
        // 创建DefaultAcsClient实例并初始化
        DefaultProfile profile = DefaultProfile.getProfile(
            "<your-region-id>",          // 地域ID
            "<your-access-key-id>",      // RAM账号的AccessKey ID
            "<your-access-key-secret>"); // RAM账号AccessKey Secret
        IAcsClient client = new DefaultAcsClient(profile);
        // 创建API请求并设置参数
        DescribeInstancesRequest request = new DescribeInstancesRequest();
        request.setPageSize(10);
        // 发起请求并处理应答或异常
        DescribeInstancesResponse response;
        try {
            response = client.getAcsResponse(request);
            for (DescribeInstancesResponse.Instance instance:response.getInstances()) {+
                System.out.println(instance.getPublicIpAddress());
            }
        } catch (ServerException e) {
            e.printStackTrace();
        } catch (ClientException e) {
            e.printStackTrace();
        }
    }
}
```
