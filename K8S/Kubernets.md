# Kubernets



## 基础学习

官方文档：https://kubernetes.io/zh/

中文文档：https://www.kubernetes.org.cn/docs





## Kubernetes 设计架构

![Kubernetes-架构](Kubernets.assets/Kubernetes-架构.jpg)

Kubernetes 分为工作节点（worker）与控制节点（master）

Kuernetes 运行应用容器必备的服务都是受 Master 管控。每个节点都有运行Docker。

核心组件：

etcd：保存整个集群的状态。

apiserver：提供资源操作的唯一入口，并提供认证、授权、访问控制、API 注册和发现等机制。

controller manager： 负责维护集群的状态，比如故障检测、自动扩展、滚动更新等。

scheduler：负责资源的调度，按照预定的调度策略将 Pod 调度到相应的机器上。

kubelet：负责维护容易的生命周期，同时也负责Volume (CVI) 和网络 （CNI）的管理。

Container runtime：负责镜像管理以及 Pod 和容器的真正运行（CRI）。

kube-proxy：负责为 Service 提供 cluster 内部的服务发现和负载均衡。



除了核心组件，还有一些推荐的 Add-ons ()



## 杂记

Kubernetes 的 Service Types 能指定你想要的网络服务类型。

提供的可选值有：

ClusterIP：这是默认值。表示这个服务只能在集群内访问，使用集群内部IP。 

NodePort：在集群内部IP的基础上，在集群的每一个节点的端口上开放此服务，可以通过 <NodeIP>:<NodePort> 地址访问此服务。

LoadBalancer：在NodePort模式的基础上，向云提供商申请LB。



Kubernetes 的三种外部访问 pod 服务方式：NodePort、LoadBalancer、Ingress

Proxy：

```shell
kubectl proxy --address=0.0.0.0 --port=8001 >/dev/null 2>&1 &
```

这样可以通过 http://localhost:8001/api/v1/namespaces/<namespace>/services/<service-name>:<port-name>/proxy 

来访问相应的服务。



