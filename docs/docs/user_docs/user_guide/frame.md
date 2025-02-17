---
sidebar_position: 1
---

# 为什么写这个框架

[美团线程池文章](https://tech.meituan.com/2020/04/02/java-pooling-pratice-in-meituan.html "美团线程池文章") 介绍中，因为业务对线程池参数没有合理配置，触发过几起生产事故，进而引发了一系列思考。最终决定封装线程池动态参数调整，扩展线程池监控以及消息报警等功能。

在开源平台找了挺多动态线程池项目，从功能性以及健壮性而言，个人感觉不满足企业级应用。

因为对动态线程池比较感兴趣，加上想写一个有意义的项目，所以决定自己来造一个轻量级的轮子。

想给项目起一个简单易记的名字，类似于 Eureka、Nacos、Redis；后和朋友商量，决定命名：**Hippo4J**。

![](https://images-machen.oss-cn-beijing.aliyuncs.com/动态线程池功能架构-1.jpg)

## 它解决了什么问题

线程池在业务系统应该都有使用到，帮助业务流程提升效率以及管理线程，多数场景应用于大量的异步任务处理。

虽然线程池提供了我们许多便利，但也并非尽善尽美，比如下面这些问题就无法很好解决。

- **原生线程池创建时无法合理评估参数问题**。比如功能使用到线程池，遇到突发流量洪峰，频繁拒绝任务。Hippo4J 提供动态修改参数功能，**避免修改线程池参数后重启线上应用**；
- 当线程池运行过程中无法再接受新的任务，此时你想知道 **线程池内线程都在做什么**？Hippo4J 提供查看线程池堆栈功能；
- 某接口频繁超时，内部依赖线程池执行，想要 **查看过去一段时间线程池运行参数情况**。Hippo4J 提供历史数据图表查看功能；
- **原生线程池无任务报警策略**。Hippo4J 内置四种报警策略，分别是：活跃度报警、队列容量报警、拒绝策略报警和运行时间过长报警。

Hippo4J 很好解决了这些问题，它将业务中所有线程池统一管理，增强原生线程池系列功能。

## 它有什么特性

应用系统中线程池并不容易管理。参考美团的设计，Hippo4J 按照租户、项目、线程池的维度划分。再加上系统权限，让不同的开发、管理人员负责自己系统的线程池操作。

举个例子，小编在一家公司的公共组件团队，团队中负责消息、短链接网关等项目。公共组件是租户，消息或短链接就是项目。

Hippo4J 除去动态修改线程池，还包含实时查看线程池运行时指标、负载报警、配置日志管理等。

- `hippo4j-adapter`：适配对第三方框架中的线程池进行监控，如 Dubbo、RocketMQ、Hystrix 等；
- `hippo4j-auth`：用户、角色、权限等；
- `hippo4j-common`：多个模块公用代码实现；
- `hippo4j-config`：提供线程池准实时参数更新功能；
- `hippo4j-console`：Web控制台前端项目；
- `hippo4j-core`：核心的依赖，包括配置、核心包装类等；
- `hippo4j-discovery`：提供线程池项目实例注册、续约、下线等功能；
- `hippo4j-example` ：示例工程；
- `hippo4j-server` ：聚合 Server 端发布需要的模块；
- `hippo4j-spring-boot`：负责与 Server 端交互的依赖组件；
- `hippo4j-tool` ：操作日志等组件代码。
