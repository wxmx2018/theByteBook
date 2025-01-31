# 2PC

二阶段提交协议（Two-phase Commit Protocol，简称 2PC）是分布式事务的核心协议, 是XA规范的实现思路。XA规范是 X/Open DTP 定义的交易中间件与数据库之间的接口规范（即接口函数），交易中间件用它来通知数据库事务的开始、结束以及提交、回滚等。目前 Oracle、MySQL 等各大数据库都提供对 XA 的支持。

在此协议中，一个事务管理器（Transaction Manager，简称 TM）协调 1 个或多个资源管理器（Resource Manager，简称 RM）的活动，所有资源管理器向事务管理器汇报自身活动状态，由事务管理器根据各资源管理器汇报的状态（完成准备或准备失败）来决定各资源管理器是“提交”事务还是进行“回滚”操作。

二阶段提交的具体流程如下：

1. 应用程序向事务管理器提交请求，发起分布式事务；
2. 在第一阶段，事务管理器联络所有资源管理器，通知它们准备提交事务；
3. 各资源管理器返回完成准备（或准备失败）的消息给事务管理器（响应超时算作失败）；
4. 在第二阶段：
	- 如果所有资源管理器均完成准备（如图 1），则事务管理器会通知所有资源管理器执行事务提交；
	- 如果任一资源管理器准备失败（如图 2 中的资源管理器 B），则事务管理器会通知所有资源管理器进行事务回滚。