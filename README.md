# Project01: CKA Exam

## Progress

Day 16 | July 25

1. 知道了编写 Pod Affinity 时要注意题目中描述的是硬限制（require）还是软限制（prefer）
2. 知道了将节点作为拓扑键（`topologyKey`）的写法和将 Zone 作为拓扑键的写法不一样，分别是 `kubernetes.io/hostname` 和 `topology.kubernetes.io/zone`
3. 使用 `kubeadm init` 完成集群安装
4. 完成 20/24 个 Scenario

Day 15 | July 24

1. 知道了 Killercoda 上要求操作 User 时，大概率不需要关心 User 的创建，直接用就可以了
2. 知道了 RoleBinding 可以用 ClusterRole，赋予 SA/User 在命名空间内的相关操作权限，这样做的意义可以将 ClusterRole 在多个命名空间复用
3. 如何实现「允许访问除了 kube-system 以外的所有命名空间的资源」这样的功能？K8s 目前是不允许的，但可以创建一个 ClusterRole 然后在各个命名空间进行 RoleBinding
4. 知道了 User 是没有命名空间的，`k auth can-i` 指定 SA 和 User 的方式也不一样，分别是 `--as=system:serviceaccount:applications:smoke` 和 `—-as=smoke`
5. 知道了 `k create` 没有 Pod
6. 知道了 Pod 可以通过配置 PriorityClass 指定调度优先级，可以通过 `k get priorityclass` 查看该资源，且它是无命名空间的
7. 完成 16/24 个 Scenario

Day 14 | July 23

1. 温习了使用 `k create` 和 `k expose` 创建 Service，两种方式差别不大，前者也能自动关联后端资源，但推荐用后者
2. 温习了创建 Ingress 的两个知识点：创建前需要检查 `k get ingressclass`；创建后需要编写 `rewrite-target`
3. 温习创建 Network Policy，注意 `spec.egress[].to` 和 `spec.egress[].ports` 是应该分开还是合并
4. 复习 RBAC，最大的困难是太慢了，操作熟练度还需要提升
5. 完成 14/24 个 Scenario

Day 13 | July 19

1. 知道了容器的配置通过 describe deployment 是看不到的；知道了 Pod 处于 Pending 状态但没有任何事件时，基本上就是调度配置的问题
2. 学会了[将 ConfigMap 挂载为 Pod 卷](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#add-configmap-data-to-a-specific-path-in-the-volume)，意思是说 ConfigMap 中每个键值对就是一个文件的文件名和内容
3. 完成了 10/24 个 Scenario

Day 12 | July 18

1. 学会了配置 vim 将 tab 转成空格，以及在可视模式下缩进多行
2. 学会了（在 /etc/kubernetes/manifests 目录下）修改 api-server 配置，以及在 api-server Pod 启动失败，无法通过 kubectl 访问集群时，查看 /var/log/pods 下的日志以定位问题
3. 学会了通过 /etc/kubernetes/manifests/etcd.yaml（中的 `--listen-client-urls` 参数）查看 etcd 监听端口
4. 完成了 3 个 Killercode [Scenario](https://killercoda.com/killer-shell-cka/)
5. 进度回顾：本周末就到就到原计划的中途点（三周）了，预习第四周完成所有 Scenario，第五周完成模拟考，第六周考试

Day 11 | July 17

1. 注册了 Killercoda，发现上面的学习资源比我想象的还多，在模拟考之前可以把 [24 个 Scenario](https://killercoda.com/killer-shell-cka) 都解决一下
2. 完成了 Scenario：[Kubelet 配置错误问题](https://killercoda.com/killer-shell-cka/scenario/kubelet-misconfigured)，知道了 kubeadm 的参数错误可能导致 kubelet 服务异常

Day 10 | July 16

1. 阅读考生手册和考试相关文档
2. 完成购买，价格 2918，很遗憾暂时没有折扣

Day 9 | July 14

1. 复习 RBAC
2. 复习网络策略，注意判断规则 or 还是 and，或者说，是一条还是两条
3. 复习恢复 etcd，为什么文档里面没提如何避免恢复时冲突这回事？
4. 完成所有 22 年真题，和 23 年重合度很高，相当于复习了一遍

Day 8 | July 11

1. 知道了可以将 sidecar 容器作为 logging agent，将 legacy 应用的日志转到标准输出，从而可以通过 kubectl logs 查看日志
2. 学会了使用 `kubectl top po --sort-by='memory’` 查看 pod 的资源占用
3. 知道了节点出现 NotReady 状态时需要检查 kubelet 服务状态
4. 完成了所有 23 年真题
5. 知道了 `kubectl edit —-save-config` 可以保存更改到 annotation，如果题目要求不能忽视了

Day 7 | July 9

1. 知道了准入控制（admission control）是 kube-apiserver 组件中的东西，有点类似中间件的概念，在 K8s API 请求认证完成后、对象持久化之前执行。K8s 默认启用的准入控制器中有一个 DefaultStorageClass，它检查未指定 `storageClass` 的新创建的 PVC，给它们指定默认值（如果有的话）
2. 知道了 PVC 的 `storageClass` 只有在 PV 没有的时候才会生效，否则它必须与 PV 保持一致
3. 给 StorageClass 对象加上 `storageclass.kubernetes.io/is-default-class: "true"` 标注标记它为默认，如果有多个 StorageClass 被标记为默认，则 DefaultStorageClass 准入控制器会在创建 PVC 时报错
4. 被编译到 kube-apiserver 二进制中的这些准入控制器被称作 compiled-in admission plugins，K8s 还支持动态的、作为扩展的 admission webhooks
5. 知道了 StorageClass 的绑定模式 `volumeBindingMode: WaitForFirstConsumer` 让 PVC 在 Pod 调度的时候创建，所以如果 Pod 通过 `nodeName` 绕过调度器会导致 PVC 一直 pending（如果通过标签指定节点亲和性就没问题）
6. 知道了 StorageClass 的 `provisioner` 指定了使用哪个云厂商的卷，若设置为 `kubernetes.io/no-provisioner` 则是本地卷
7. 完成了第 13 题 PVC 创建，意识到官方文档的搜索不能很好地检索到 Tasks 部分的内容，或许可以熟悉一下这块章节方便之后自己查阅

Day 6 | July 8

1. 完成了第 7 题七层负载，知道了创建 ingress 时需要注意的一点是 `pathType`，一般用 `Prefix`，用 `—-rule` 创建的话需要加 * 号
2. 知道了 traefik 不支持 `nginx.ingress.kubernetes.io/rewrite-target`
3. 完成第 8 题 Deployment 扩展
4. 完成第 9 题节点亲和性，第 10 题检查可用节点数量
5. 文档检索完成：第 11 题创建多容器 Pod，第 12 题创建 hostPath 类型的持久卷

Day 5 | July 6

1. 整理了常用 alias 别名
2. 在 k3s 上验证了 NetworkPolicy
3. 完成了第六题创建 Deployment 和 NodePort Service（四层）

Day 4 | July 5

1. 阅读网络策略文档，了解 NetworkPolicy 资源需要由 CNI 应用生效，策略类型(policyTypes)有出口 egress 和入口 ingress 两类，作用的对象可以是 Pod，命名空间或 IP block。如果要允许 Pod A 访问 Pod B，那要同时开启 A 的 egress 和 B 的 ingress
2. 不配置网络策略的情况下，默认都是允许的，但一旦配置了一种策略类型，就会启用白名单模式。因为是白名单，所有网络策略不会出现冲突。如果显式指定允许所有入口流量（`spec.ingress: - {}`），那再添加入口限制规则也不会生效
3. policyTypes 如果置空，那么默认：1）总是会开启 ingress 策略，若没有配置 ingress 规则，则会限制所有入口流量；2）如果配置了 egress 规则则会开启
4. 知道了 spec.ingress.from 或 spec.egress.to 下面是规则列表，而且 namespaceSelector 和 podSelector 可以是写在不同的规则里，也可以是组合在一条规则里，注意区分
5. 注意 IP block 应该只用于集群外的地址，因为 Pod 的 IP 是变化的
6. NetworkPolicy 都是用于四层流量，不一定包括 ICMP
7. 知道了网络策略中的 namespaceSelector 不能直接指定命名空间名称，可以给命名空间加上标签，或者也可以用 `matchLabels: kubernetes.io/metadata.name: my-ns`，命名空间都会自带这样的不可变标签

Day 3 | July 3

1. 学会了通过 `apt update && apt-cache madison kubeadm` 查询安装包版本
2. 知道了 kubeadm upgrade apply 默认会升级 etcd，不符合题目要求，需要加选项禁止，可见执行重要命令时都有必要先读读帮助文档
3. 知道了控制平面节点上的 API server 是作为静态 Pod 运行的，配置文件位于 /etc/kubernetes/manifests，将它们临时移出，kubectl 就会自动停止 API server 的 Pod
4. 完成第四题，学习了 etcd 的备份和还原，知道了还原之前需要先停止所有 api-server 服务
5. 知道了 /etc/kubernetes/manifests 目录中也包含 etcd 配置，而且为了避免数据冲突，etcd 数据目录需要换成新的，因此在恢复 API server 前需要修改 etcd.yml

Day 2 | July 2

1. 实践了 drain 和 cordon，知道了前者包含后者
2. 发现一处中文文档翻译缺陷，为了避免误解和更好理解，还是应该看英文文档
3. 知道了 kubeadm 只是 k8s 的安装方式之一，但目前只需要考虑它，升级集群也是从它开始
4. 知道了升级 k8s 节点的四个步骤：1）drain；2）`apt install kubeadm=xx kubelet=xx kubectl=xx`；3）`kubeadm upgrade apply`；4）`kubectl uncordon`
5. 知道了有时也可以用新节点替换的方式升级节点，drain 之后就不需要再 uncordon，可能比直接升级原节点更简单
6. 知道了 apt-mark unhold/hold 是控制 ubuntu 上的安装包是否要自动升级，如果 apt install 升级不了的话就需要 unhold

Day 1 | June 30

1. 开始我的第一个项目——考取 CKA 证书；确定第一步目标：两周内完成 2023 年真题
2. 体验了 kubectl config 各个命令，知道了 kubeconfig 里面有 cluster, context, user 等概念
3. 手打了 kubectl create clusterrole 命令，完成第一道题 RBAC，easy
4. 知道了其实也不用记住所有细节，知道如何搜索手册就行
5. 学习了 drain，cordon 等概念
6. 知道了 Pod 干扰预算（pod disruption budget）是为了避免自愿干扰导致过多 Pod 被销毁，开启了 PDB 后，删除资源不应该通过 kubectl delete，而应该是更安全的遵循 PDB 的 API
7. 知道了 drain 命令是遵循 PDB 的，所以可能会阻塞
