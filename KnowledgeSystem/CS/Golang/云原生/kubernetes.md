
Kubernetes: 一个大规模的容器编排系统

- 一个kubernetes集群是由一组被称作结点（Node）的机器组成
- Pod：运行中的一组容器，Pod是kubernetes中应用的最小单位。

特性
- 服务发现：能够知道自己的哪些服务是在运行的，哪些服务是崩掉了的。
- 负载均衡：负载均衡的分配网络流量，从而使部署稳定


- 金丝雀部署=灰度发布：先发布一部分，程序稳定，再发布一部分（正式服，体验服）




组件架构
- kubernetes cluster：集群
- Node：服务器结点
- controller manager：决策者
- etcd：kv存储库
![[Pasted image 20220805165543.png]]
特点
- 所有组件的交互必须通过api-server
- 集群里的网络访问都是通过kube-proxy
- 运行的所有应用程序，都需要一个容器运行时环境（docker）。
- 每一个结点都需要一个监工、厂长（kubelet），结点应用的变化及时通知决策层（api-server）
![[Pasted image 20220805172304.png]]



![[Pasted image 20220807170409.png]]



# Deployment
==deployment用于管理一个或多个pod，把一个服务（pod）备份到多个节点上==

- 一个pod可以包含多个容器。
- 一个集群含多个结点，一个结点就是一个机器。
- deployment拥有自愈能力，删除pod之后还会再任意一个结点新建一个pod。除非删除deploy，所有pod才会删除。
- deployment用于管理一个或多个pod
- deployment可以启动多副本（replicas），可以扩容（scale），缩容（scale）
- deployment是滚动更新（不停机更新）
- deployment版本回退功能
	- rollout history (查看历史记录)
	- rollout undo(版本回退)

![[Pasted image 20220808112329.png]]

# Service
以下三个pod是同一个服务部署在不同结点上，也称为一组服务。
通过Service可以将这一组服务统一公开给网络服务。

![[Pasted image 20220808145910.png]]
- Service：将一组Pods公开为网络服务的抽象方法。Pod的服务发现与负载均衡。(集群内访问)
	- 集群内使用service的ip:port就可以负载均衡的访问。
		- api地址：serviceIP:8000
		- api地址：service域名:8000
- NodePort：集群外也可以访问

- `kubectl expose deploy my-dep --port=8000 --target-port=80 --type=ClusterIP`
- `kubectl expose deploy my-dep --port=8000 --target-port=80 --type=NodePort`
- `kubectl get service`



# ingress
- ingress是service的统一网关入口
- service是pod的网关入口，service负载均衡pod

service的port监听pod的target port。




-----------------------------
# sealan k8s
![[Pasted image 20220809151656.png]] 

- 应用部署在Node上、一台机器是一个Node（结点）。
- 一个结点是一个服务器
- deployment：用于管理pod。
- 每个Node上需要容器运行时环境（docker）、kubelet（厂长）、kube-proxy（门卫大爷，相当于路由）
	- Node（东厂、西厂）
	- kubelet：检测各个应用是否运作，死了就及时通知api-server 
		- 负责 Kubernetes 主节点和工作节点之间通信的过程; 它管理 Pod 和机器上运行的容器。
	- kubeadm：帮机器快速搭建kubernetes
	- kubectl：向机器发号施令的
	- docker：容器运行时环境


```bash
#查看集群所有节点
kubectl get nodes

NAME            STATUS   ROLES                  AGE   VERSION
k8s-node-1-56   Ready    worker                 87d   v1.21.5
k8s-node-2-57   Ready    worker                 87d   v1.21.5
kubernetes-55   Ready    control-plane,master   87d   v1.21.5


#根据配置文件，给集群创建资源
kubectl apply -f xxxx.yaml

#查看集群部署了哪些应用？
docker ps   ===   kubectl get pods -A

# 运行中的应用在docker里面叫容器，在k8s里面叫Pod
kubectl get pods -A

NAMESPACE                      NAME                                               READY   STATUS      RESTARTS   AGE
seanet-test                    minio-59c76c5565-rx7fb                             1/1     Running     0          24h
seanet-test                    nginx-v1-74b6884df4-nqhf8                          1/1     Running     0          6d3h
seanet-test                    postgre-postgresql-0                               1/1     Running     0          7d
seanet-test                    seanet-v1-7c8699d57d-ssxrj                         1/1     Running     0          5d2h


```



```bash
kubectl run mynginx --image=nginx  -n seanet-test

# 查看default名称空间的Pod
kubectl get pod -n seanet-test
# 描述
kubectl describe pod 你自己的Pod名字
# 删除
kubectl delete pod Pod名字 -n seanet-test
# 查看Pod的运行日志
kubectl logs Pod名字 -n seanet-test

# 每个Pod - k8s都会分配一个ip
kubectl get pod -owide
# 使用Pod的ip+pod里面运行容器的端口
curl 192.168.169.136

# 集群中的任意一个机器以及任意的应用都能通过Pod分配的ip来访问这个Pod
```





##  k8s架构
![[Pasted image 20220809152219.png]]


## k8s集群部署
![[Pasted image 20220809155014.png]]






# postgre
```bash
su - postgres

psql -U admin




```