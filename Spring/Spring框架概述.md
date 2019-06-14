# Spring框架概述

Version 5.1.7RELEASE

Spring简化了企业级应用开发的难度。提供了在企业环境中使用Java所需要的一切，支持Groovy 和Kotlin作为JVM上的替代语言，同时可以灵活的根据应用程序的需求创建不同的体系架构。

Spring5.1要求JDK的版本至少为1.8，同时对JDK11 LTS提供开箱即用的支持。

Spring是开源的，拥有庞大且活跃的社区，社区基于现实世界中的各种用例持续提供反馈，这有助于Spring在很长一段时间内健康发展。

## 1.我们所说的Spring是指什么？

Spring在不同的环境下代表不同的东西，它可以指代Spring框架本身，即一切开始的地方。

随着时间的推移，其他Spring项目都构建于Spring框架之上，特别的，当人们说Spring的时候，代表的是整个项目系列。

Spring框架分为几个模块。应用可以选择自己需要的模块，最核心的模块是核心容器模块，包括模型管理和依赖注入机制。除此之外，Spring框架对不同的应用架构提供了基础支持，包括消息服务，事务控制和数据持久化，Web开发。包含基于Servlet的SpringMvc框架，以及Spring WebFlux响应式web框架。

## 2.Spring和Spring框架的历史

Spring作为复杂的J2EE规范的挑战者，诞生于2003年。有些人认为两者是竞争关系，但Spring实际上是对J2EE的补充，Spring的编程模型中并不包含J2EE平台规范，相反，Spring集成了从J2EE旗下精心挑选的一些规范。

- Servlet API(JSR 340)
- WebSocket API(JSR 356)
- Concurrency Utilities (JSR 236)
- JSON Binding API (JSR 367)
- Bean Validation(JSR 303)
- JPA (JSR 338)
- JMS (JSR 914)
- 必要时用于事务协调的JTA / JCA设置

Spring框架同样支持依赖注入（DI) (JSR 330)和常用注解（JSR 250)规范,开发者们可能会选择使用这些规范而不是Spring提供的特定机制。

## 3.Spring设计哲学

当你在学习一个框架的时候，重要的是不仅要知道它能做什么，还要知道它遵循什么样的原则。

- 在各个层面提供选择：Spring允许您尽可能的推迟设计决策。例如，可以通过配置来切换持久化功能提供者，而不需要修改代码。对于其他基础架构以及第三方API的集成也是如此。
- 保持向后兼容性：Spring尽量避免在版本升级时，引入大变动。
- 关注API设计：Spring团队花费了大量时间去思考如何使API看起来直观，并且可以保持多个版本和年份。
- 为代码质量设定高标准：Spring框架提倡编写有意义，最新，准确的javadoc。它是极少数可以具有清晰的代码结构且包之间没有循环依赖关系的项目之一。

