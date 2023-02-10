---
marp: true
header: ''
footer: 'by renlu <https://404.ms/>'
---
<!-- paginate: true -->
<style>
footer {
    text-align:right;
    display:inline-block;
    
}
table {
width:80%;
min-width:80%;
}
</style>
# why Java


First question

---

# why Java 

![https://404.ms/file/xurenlu202202/37814200.1001.png](https://404.ms/file/xurenlu202202/37814200.1001.png)

---

# Why Java

![https://404.ms/file/xurenlu202202/29369300.5301.png](https://404.ms/file/xurenlu202202/29369300.5301.png)

---
# php vs java

| PHP | Java |
| --- | --- |
| 多进程模型 | 多线程模型 |
| Spaghetti code | class构成的世界 |
| 解释型语言 | 编译型语言 |
| 弱类型 | 强类型 |
| Web mostly | everywhere |
---
### Spaghetti code
<style >
img {
display:inline-block;
}
</style>
  ![面条代码](https://404.ms/file/xurenlu202202/53158900.7840.png)


--- 
# glance of Spring Boot

准备工作 
- Mac
- [IDea](https://www.jetbrains.com/zh-cn/idea/)
- MySQL Server

---
第一步，创建工程
![https://404.ms/file/xurenlu202202/46086700.7919.png](https://404.ms/file/xurenlu202202/46086700.7919.png)

---
![](https://404.ms/file/xurenlu202202/86925100.2819.png)

---
![https://404.ms/file/xurenlu202202/21311800.4707.png](https://404.ms/file/xurenlu202202/21311800.4707.png)

---
## 知识点

    Plz do check the help.md!

- Idea
    - plugin management
    - coding guideline scanner
    - run/debug configuration    
- Spring boot initializr
- lots of spring starter 
- lombok
- profiles (dev/prod/default)
- hotswap 
- maven


---
## Toc
1. 类加载器
2. 内存回收
3. Maven和依赖管理
4. 单元测试


---
## Toc 
1. 持续集成
2. 日志管理
3. 编码规约插件
4. 设计模式
5. Swagger
6. Spring Cloud
---

## 语言基础
Java 7 vs 8 ，Java的基础概念(基本数据类型、对象) String 泛型 ,依赖注入,NullPointer,强类型vs 弱类型
Collections类：ArrayList LinkedList HashTable HashMap Set Vector
Hash碰撞，Hash函数
多线程、synchronized、悲观锁、乐观锁、原子操作、线程池、装箱
[https://github.com/Snailclimb/JavaGuide](https://github.com/Snailclimb/JavaGuide)

---

### java有着php世界所没有的各种通行约定或规范
- 源代码有典型的目录结构
![https://ylpicture.oss-cn-beijing.aliyuncs.com/201906/33174600.5006.png](https://ylpicture.oss-cn-beijing.aliyuncs.com/201906/33174600.5006.png)

- 发布遵循通用标准:Jar包与war包
- 一般自动运行单元测试后进行打包

---

### 内存回收篇

-  引用计数和可达性算法
-  Out Of Memory
-  Stack Overflow
-  Stop-the-world

---
### Maven

包坐标系:GAV和scope

maven命令:mvn [plugin-name]:[goal-name]
```mvn clean package docker:build```

三大主要模块：

	- plugin 
	- dependency 
	- property

常用仓库:[https://mvnrepository.com](https://mvnrepository.com)

---
### 单元测试 
核心包应做到接近100%的代码覆盖
TDD
JUnit
Mock

---
### 为支持测试所做的进化
相对PHP,对代码常见的改造
```php
class ApiClient{
	function getOrders(){
    	$url = $this->buildOrdersUrl();
        $content = Http::requestURL($url);
        $orders = $this->parseOrders($content):
        return $orders;
    }
}

$apiClient = new ApiClient;
$orders = $apiClient->getOrders();
```

---
### 为支持测试所做的进化
```php
Class ApiClient {
	$httpClient;
	public  getOrders(){
	$url = $this->buildOrdersUrl();
        $content = $httpClient->requestURL($url);
        $orders = $this->parseOrders($content):
        return $orders;
    }
}

```

---

### 为支持测试所做的进化
测试代码

```java
Class TestApiClient {
	public void testGetOrders(){
    	HttpClient httpClient = mock(HttpClient.class);
        when(httpClient.requestUrl(anyString())).thenReturn("{'status':200,'data':'......省略3000字......'}");
        ApiClient apiClient = new ApiClient();
        apiClient.setHttpClient(httpClient);
		Orders orders = apiClient.getOrders();
        ...这里不再写测试相关的逻辑;
    }
}
```


---
### 持续集成
Jenkins
[https://www.ibm.com/developerworks/cn/java/j-lo-jenkins/index.html](https://www.ibm.com/developerworks/cn/java/j-lo-jenkins/index.html)

---
###  日志框架
common-logging
slf4j
log4j
logback
log4j-over-slf4j

---

## 日志框架
![https://ylpicture.oss-cn-beijing.aliyuncs.com/201906/53116700.2878.png](https://ylpicture.oss-cn-beijing.aliyuncs.com/201906/53116700.2878.png)

---
### 编码规约

阿里巴巴Java开发规约
[https://github.com/alibaba/p3c/](https://github.com/alibaba/p3c/)

---
### 设计模式 
图说设计模式:
[https://design-patterns.readthedocs.io/zh_CN/latest/](https://design-patterns.readthedocs.io/zh_CN/latest/)

---
### Spring cloud
- 安装Idea插件:Spring assistant
- 指定编译目标版本
- docker插件初始化 spring-web-starter
