# Project01: CKA Exam

Day 26 | August 10

1. 顺利完成考试！题目比模拟考简单，时间很宽裕，做完还剩半小时，又检查了一遍，有信心冲满分 ;D
2. 可以配合 Better Display 使用外接屏幕，只是整个过程连接不能断开，否则 PSI 会退出要重新开始检测，所以需要确保视频线连接稳定且不要太短，能够让你端着笔记本扫视四周。使用 24 寸显示器真的让考试轻松很多！
3. 不能佩戴手表，可以看远程桌面里的时间（有时差）

Day 25 | August 9

1. 6 分钟完成 Scenario "RBAC User Permissions"
2. 9 分钟完成 "Multi Containers and Pod shared Volume"，已经尽力了
3. 验证发现 `k delete --force --grace-period=0` 的确比 `k delete --now` 更快，所以 `export now="--force --grace-period 0"` 还是有必要的

Day 24 | August 8

1. 阅读考试注意事项，知道了有可能不让戴手表，要做好不能使用计时器的准备，想别的办法控制时间，不要浪费时间在不熟悉的题目上
2. 阅读 Reddit 上的考试经验分享，知道了虽然会检查麦克风，但和考官只有文字交流（不用担心口语和听力了哈哈哈）
3. 考试前的检查可能要个把小时，可能要检查不止一次，心态上做好准备
4. 预约了周六早上 6 点考试
5. 下载了 PSI Browser 并运行了检测，实践发现使用镜像扩展屏会被判定为 2 个显示器，但是用 BetterDisplay 关闭内置屏幕就通过检测了，建议把自动识别打开，非常好用！
6. 国内环境需要代理，否则会提示网速过慢无法进行考试

Day 23 | August 7

1. 学会了使用 kubeadm certs 命令查看证书过期日期和更新证书
2. 收获 etcd 快照恢复经验：1）不需要 drain node；2）执行恢复前要确保 API Server 容器都停止了，可能要等好几分钟，用 `watch crictl ps` 命令观察；3）虽然文档没有提及，但其实和备份一样也需要提供证书等参数；4）需要修改 etcd.yaml 中 hostPath 挂载目录，注意不是命令参数

Day 22 | August 6

1. 完成第二次模拟考
2. 知道了当问你要用什么命令的时候，不能用 alias，虽然环境配了 `k=kubectl`，但答案中用 `k` 会判定错误
3. 知道了将 /etc/kubernetes/manifests 下的文件从 xx.yaml 重命名为 xx.yaml.bak 并不会导致静态 Pod 消失，得移出这个目录才行
4. controlplane node 上可能有 taint，所以当让你调度 Pod 到 worker 节点，这个是自动的
5. 知道了 `k events` 不是唯一的查看事件的方式，还有 `k get events`，两者可用的选项也不一样，前者可以用 `--type` `--for` 等，后者可以用 `--sort-by`
6. 知道了 `k delete` 是有 `--now` 选项的，等同 `--grace-period=1`
7. 知道了 kubelet 起不来，错误提示缺少 kubelet config.yaml 文件，是因为还没有加入集群，加入就好了

Day 21 | August 4

1. 计时完成一道优先级调度 Pod 用时 5 分钟，完成一道 NetworkPolicy 用时 8 分钟，决定将 8 分钟作为第一轮每道题用时上限
2. RBAC 失误，需要做 ClusterRoleBinding 的时候做成了 RoleBinding，这类题做完后需要标记，如果有空余时间再用 `k auth can-i` 验证
3. 4 分钟完成 Anti-Affinity；6 分钟完成 DaemonSet with HostPath；3 分钟完成 debug multiple container Deployment
4. 15 分钟完成 Ingress Create，查 ingress class name 字段找了很久，查文档没查到不能着急，不要翻页找，调整关键词，高亮滚动条

Day 20 | August 1

继续分享考试 tips：

1. 记住常用语法，比如节点标签、磁盘挂载和资源申请
2. Terminal 设置：Edit → Preferences
    1. General 面板最下面，开启自动复制
    2. Advanced 面板，勾选 "Disable all menu access keys"，这样就可以用 readline 操作了，比如 alt f/b 来按词跳光标
3. Pod 模版用 `k run` 获取，Deployment 模版用 `k create deploy` 获取，这样是最快的，DaemonSet 等 kubectl 没有的再查文档
4. 浏览器设置：翻到最下面，勾选 "always show scrollbar"，搭配网页查询勾选 "highlight"，方便搜索定位
5. 知道了 Service CIDR 要在 API Server manifest 中查看
6. 知道了在节点 /etc/cni/net.d 目录下可以查看使用的 CNI plugin
7. 知道了考试环境下的 etcd 操作用 etcdctl 而不是更新的 etcdutl，不用关心 deprecated command 提示
8. [etcd 备份文档](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster)中提供了 3 种方法，如果直接用第一种 built-in snapshot，命令看起来能执行，但实际上不对，需要加 option 提供 ca 证书，而具体连接信息可以通过 API Server manifest 查看
9. 没有外置摄像头，笔记本屏幕太小，但又不允许使用扩展屏，怎么办？在 Reddit 得知一个 mac 小工具 [BetterDisplay](https://github.com/waydabber/BetterDisplay)，可以关闭自带屏幕，完美符合考试需要！

Day 19 | July 31

1. 阅读[远程桌面使用须知](https://itnext.io/cks-cka-ckad-changed-terminal-to-remote-desktop-157a26c1d5e)，知道了考试环境不可以直接访问终端使用的文件系统，但可以使用 Mousepad Editor，然后复制到 vim。复制前一定要设置好 vim 缩进，而考试环境是自动设置好的
2. 阅读监考平台 [PSI Bridge 须知](https://training.linuxfoundation.org/blog/update-on-certification-exam-proctoring-migration/?utm_source=lftraining&utm_medium=twitter&utm_campaign=blog)，知道了只能用一个显示器
3. 完成了第一次模拟考（以下分享经验）
4. Mousepad Editor 需要设置缩进使用 2 个空格，默认是 8
5. 过于依赖频繁查询文档会浪费很多时间，建议额外开一个 editor tab 用于存放常用的模版，避免重复查询
6. 远程桌面翻页不方便，浏览器可以用 page up/down，终端可以用 shift + page up/down
7. 按大小写键会切换到全角标点符号输入，注意避免
8. 没有尝试出有效的窗口切换快捷键，需要排列好窗口
9. 知道了创建 NodePort Service 后可以通过 `k get ep` 查看 Endpoint，并可在相应的节点上访问这个内部 IP
10. 知道了可以通过 `kubectl api-resources` 查看 API 资源
11. 统计资源个数的时候，需要注意用 `k get xx --no-header`，否则会多一行
12. 不管是直接用 vim 还是用 Mousepad Editor 编辑好再拷贝，效率都不太高，建议像下面这样编写 here document，每个题一份，修改完直接全选复制粘贴运行就可以了，非常高效！把它记下来

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
EOF
```

Day 18 | July 30

1. 阅读文档《[Troubleshooting Clusters](https://kubernetes.io/docs/tasks/debug/debug-cluster/)》，知道了如果 K8s 不是基于 systemd 运行，应该从 /var/log/ 目录下查看 API Server, Scheduler, kubelet 等组件的日志
2. 知道了 K8s 可以通过 RuntimeClass 提供多份容器运行时配置，在 Pod 的 `spec.runtimeClassName` 中指定，就可以使其跑在不同的运行时下
3. 知道了 RuntimeClass 可以指定 Pod Overhead 开支，作为 ResourceQuota 的补充，也可以指定内存和 CPU 用量，在调度时都会考虑
4. 复习了 Day 1-14 笔记
5. 意识到考试前做 20-30 分钟运动非常有必要，对更好地发挥将会很有帮助

Day 17 | July 26

1. 温习了 kubeadm 升级、加入集群，知道了只有在还原 etcd 时才需要停止 api-server（移出 /etc/kubernetes/manifests 中的静态 Pod），只是升级是不需要的
2. 知道了静态 Pod 是由 kubelet 直接管理的，不受 api-server 控制，但后者还是能看到它，名字会加上节点名后缀。静态 Pod 不能访问 API 对象，包括 ServiceAccount, ConfigMap, Secret 等
3. 完成 24 个 Scenario！

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
5. 知道了 Pod 可以通过配置 PriorityClass 指定调度优先级，可以通过 `k get priorityclass` 查看该资源，且它是无命名空间的
6. 完成 16/24 个 Scenario

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

# Project00: Open Source Contributions

## 04) [ko: e2e test](https://github.com/ko-build/ko/pull/1350), 06/29

1. Learned how to write a e2e test
2. Enabled ko to support the production of smaller images
3. Made a contribution to a project with 7.3k star and closed this issue with 9 discussions

## 03) [goreleaser: facilitate issue closure](https://github.com/goreleaser/goreleaser/issues/4245#issuecomment-2153648095), 06/07

## 02) [godu: fix unicode display](https://github.com/viktomas/godu/pull/96), 05/22

1. Enabled this disk cleanup tool to support the display of directories/files with Unicode name
2. Learned how to handle the rendering of multi-byte characters(runes) in cell-based view

## 01) [godu: cli help documentation](https://github.com/viktomas/godu/pull/95), 05/13