#k8s核心对象概念
##基本对象概念
###基本对象（偏底部，对用户操作比较少）
**Pod**  
Pod是最小部署单元，一个Pod有一个或者多个容器组成，Pod中容器共享存储和网络，在同一台Docker主机上运行。  
**Service**  
Service一个应用服务抽象，定义了Pod逻辑集合和访问这个Pod集合的策略。  
Service代理Pod集合对外表现是为一个访问入口，分配一个集群IP地址，来自这个IP的请求将负载均衡转发后端Pod中的容器。  
Service通过Label Selector选择一组Pod提供服务。  
**Volume**  
数据卷，共享Pod中容器使用的数据。  
可能在集群外，防止Pod重启或者故障文件丢失，一般做持久化。  
**Namespace**  
命名空间将对象逻辑上分配不同Namespace，可以是不同的项目、用户等区分管理，并设定控制策略，从而实现多租户。  
命名空间也称为虚拟集群。  
**Label**  
标签用于区分对象（比如Pod、Service）,键/值对存在；每个对象可以有多个标签，通过标签关联对象。  
### 基于基本对象更高层次抽象 ###
**ReplicaSet**  
下一代Replication Controller，确保任何给定时间指定的Pod副本数量，并提供声明式更新等功能。  
RC与RS唯一区别：label selector支持不同，RS支持新的基于集合的标签，RC仅支持基于等式的标签。  
**Deployement**  
Deployement是一个更高层次的API对象，他管理RS和Pod，并提供声明式更新等功能。  
官方建议使用Deployement管理RS，而不是直接使用RS，这就意味着可能永远不需要直接操作RS对象。  
**StaefulSet（Null）**  
有状态的部署，比如Zookeeper  
StatefulSet适合持久性的应用程序，有唯一的网络标识符（IP），持久存储，有序的部署、扩展、删除和滚动更新。  
**DaemonSet（Null）**  
比如监控，每个节点部署一个  
DeamonSet确保所有（或一些）节点运行同一个Pod。当节点加入Kubernetes集群中，Pod会被调度到该节点上运行，当节点从集群中移除时，DaemonSet的Pod会被删除，删除DaemonSet会清理他所有创建的Pod。  
**Job（Null）**
一次性任务，运行完成后Pod’销毁，不再重新启动新容器，还可以任务定时运行