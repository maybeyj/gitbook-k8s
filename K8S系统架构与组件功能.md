#K8S系统架构与组件功能

![](https://i.imgur.com/HuFlBff.png)
kubectl：用户操作命令  
##Master组件 
**kube-apiserver**  
kubernetes api，集群的统一入口，各组件协调者，以HTTP API提供接口服务，所有对象资源的增删改查和监听操作都交给APIServer处理后再提交给Etcd存储。  
**kube-controller-manager**  
处理集群心中常规后台任务，一个资源对应一个控制器，而ControllerManager就是负责管理这些控制器的。  
**kube-scheduler**
根据调度算法为新创建的Pod选择一个Node节点。  
##Node组件
**kubelet**  
是Master在Node节点上的Agent，管理本机运行容器的生命周期，比如创建容器、Pod挂载数据卷、下载secret、获取容器和节点状态等工作。  
kubelet将每个Pod转换成一组容器。  
**kube-proxy**  
在Node节点上实现Pod网络代理，维护网络规划和四层负载均衡工作。
**docker/rocket/rkt**
运行容器
##第三方服务
**Etcd**  
分布式键值存储系统，用于保持集群状态，比如Pod、Service等对象信息。