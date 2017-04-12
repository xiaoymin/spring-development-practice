# spring （由Rod Johnson创建的一个开源框架） {#xiaoymin}

Spring是一个开源框架，Spring是于2003 年兴起的一个轻量级的Java 开发框架，由Rod Johnson创建。简单来说，Spring是一个分层的JavaSE/EEfull-stack\(一站式\) 轻量级开源框架。

# 起源

你可能正在想“Spring不过是另外一个的framework”。当已经有许多开放源代码（和专有）J2EEframework时，我们为什么还需要Spring Framework？

Spring是独特的，因为若干个原因：

它定位的领域是许多其他流行的framework没有的。Spring致力于提供一种方法管理你的业务对象。

Spring是全面的和模块化的。Spring有分层的体系结构，这意味着你能选择使用它孤立的任何部分，它的架构仍然是内在稳定的。因此从你的学习中，你可得到最大的价值。例如，你可能选择仅仅使用Spring来简单化JDBC的使用，或用来管理所有的业务对象。

它的设计从底部帮助你编写易于测试的代码。Spring是用于测试驱动工程的理想的framework。

Spring对你的工程来说，它不需要一个以上的framework。Spring是潜在地一站式解决方案，定位于与典型应用相关的大部分基础结构。它也涉及到其他framework没有考虑到的内容。

# 背景

Rod Johnson在2002年编著的《Expert one on one J2EE design and development》一书中，对Java EE 系统框架臃肿、低效、脱离现实的种种现状提出了质疑，并积极寻求探索革新  
之道。以此书为指导思想，他编写了interface21框架，这是一个力图冲破J2EE传统开发的困境，从实际需求出发，着眼于轻便、灵巧，易于开发、测试和部署的轻量级开发框架。Spring框架即以interface21框架为基础，经过重新设计，并不断丰富其内涵，于2004年3月24日，发布了1.0正式版。同年他又推出了一部堪称经典的力作《Expert one-on-one J2EE Development without EJB》，该书在Java世界掀起了轩然大波，不断改变着Java开发者程序设计和开发的思考方式。在该书中，作者根据自己多年丰富的实践经验，对EJB的各种笨重臃肿的结构进行了逐一的分析和否定，并分别以简洁实用的方式替换之。至此一战功成，Rod Johnson成为一个改变Java世界的大师级人物。

传统J2EE应用的开发效率低，应用服务器厂商对各种技术的支持并没有真正统一，导致J2EE的应用没有真正实现Write Once及Run Anywhere的承诺。Spring作为开源的中间件，独立于各种应用服务器，甚至无须应用服务器的支持，也能提供应用服务器的功能，如声明式事务、事务处理等。

Spring致力于J2EE应用的各层的解决方案，而不是仅仅专注于某一层的方案。可以说Spring是企业应用开发的“一站式”选择，并贯穿表现层、业务层及持久层。然而，Spring并不想取代那些已有的框架，而是与它们无缝地整合。

# 特性

强大的基于 JavaBeans的采用控制反转（Inversion of Control，IoC）原则的配置管理，使得应用程序的组件更加快捷简易。

一个可用于从 applet 到 Java EE 等不同运行环境的核心 Bean 工厂。

数据库事务的一般化抽象层，允许宣告式\(Declarative\)事务管理器，简化事务的划分使之与底层无关。

内建的针对 JTA 和 单个 JDBC 数据源的一般化策略，使 Spring 的事务支持不要求 Java EE 环境，这与一般的 JTA 或者 EJB CMT 相反。

JDBC 抽象层提供了有针对性的异常等级\(不再从SQL异常中提取原始代码\), 简化了错误处理, 大大减少了程序员的编码量. 再次利用JDBC时，你无需再写出另一个 '终止' \(finally\) 模块. 并且面向JDBC的异常与Spring 通用数据访问对象\(Data Access Object\) 异常等级相一致.

以资源容器，DAO 实现和事务策略等形式与 Hibernate，JDO 和 iBATIS SQL Maps 集成。利用众多的反转控制方便特性来全面支持, 解决了许多典型的Hibernate集成问题. 所有这些全部遵从Spring通用事务处理和通用数据访问对象异常等级规范.

灵活的基于核心 Spring 功能的 MVC 网页应用程序框架。开发者通过策略接口将拥有对该框架的高度控制，因而该框架将适应于多种呈现\(View\)技术，例如 JSP，FreeMarker，Velocity，Tiles，iText 以及 POI。值得注意的是，Spring 中间层可以轻易地结合于任何基于 MVC 框架的网页层，例如 Struts，WebWork，或 Tapestry。

提供诸如事务管理等服务的面向切面编程\(AOP\)框架。

