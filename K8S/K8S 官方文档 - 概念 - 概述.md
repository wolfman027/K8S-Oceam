> 概述
>
> https://kubernetes.io/zh-cn/docs/concepts/overview/

**Kubernetes** 这个名字源于希腊语，意为 **舵手** 或 **飞行员**。

K8s 这个缩写是因为 K 和 s 之间有 8 个字符的关系。



## 为什么需要 Kubernetes，它能做什么？

容器是打包和运行应用程序的好方式。

在生产环境中， 你需要管理运行着应用程序的容器，并确保服务不会下线。 例如，如果一个容器发生故障，则你需要启动另一个容器。 如果此行为交由给系统处理，是不是会更容易一些？

Kubernetes 为你提供了一个**可弹性运行分布式系统的框架**。 

Kubernetes 会满足你的**扩展要求**、**故障转移你的应用**、**提供部署模式**等。 

例如，Kubernetes 可以轻松管理系统的 Canary (金丝雀) 部署。



K8S 为你提供：

- 服务发现和负载均衡

  Kubernetes 可以使用 DNS 名称或自己的 IP 地址来暴露容器。 如果进入容器的流量很大， Kubernetes 可以负载均衡并分配网络流量，从而使部署稳定。

- 存储编排

  Kubernetes 允许你自动挂载你选择的存储系统，例如本地存储、公共云提供商等。

- 自动部署和回滚

  你可以使用 Kubernetes 描述已部署容器的所需状态， 它可以以受控的速率将实际状态更改为期望状态。 

  例如，你可以自动化 K8S 来为你的部署创建新容器， 删除现有容器并将它们的所有资源用于新容器。

- 自动完成装箱计算

  你为 Kubernetes 提供许多节点组成的集群，在这个集群上运行容器化的任务。 

  你告诉 Kubernetes 每个容器需要多少 CPU 和内存 (RAM)。 Kubernetes 可以将这些容器按实际情况调度到你的节点上，以最佳方式利用你的资源。

- 自我修复

  Kubernetes 将重新启动失败的容器、替换容器、杀死不响应用户定义的运行状况检查的容器， 并且在准备好服务之前不将其通告给客户端。

- 密钥与配置管理

  Kubernetes 允许你存储和管理敏感信息，例如密码、OAuth 令牌和 SSH 密钥。 

  你可以在不重建容器镜像的情况下部署和更新密钥和应用程序配置，也无需在堆栈配置中暴露密钥。

- 批处理执行

  Kubernetes 还可以管理你的批处理和 CI（持续集成）工作负载，如有需要，可以替换失败的容器。

- 水平扩容

  使用简单的命令、用户界面或根据 CPU 使用率自动对你的应用进行扩缩。

- IPv4/IPv6 双栈

  为 Pod（容器组）和 Service（服务）分配 IPv4 和 IPv6 地址。

- 为可扩展性设计

  在不改变上游源代码的情况下为你的 Kubernetes 集群添加功能。



## Kubernetes 不是什么

Kubernetes 不是传统的、包罗万象的 PaaS（平台即服务）系统。 

由于 Kubernetes 是在容器级别运行，而非在硬件级别，它提供了 PaaS 产品共有的一些普遍适用的功能， 例如部署、扩展、负载均衡，允许用户集成他们的日志记录、监控和警报方案。 但是，Kubernetes 不是单体式（monolithic）系统，那些默认解决方案都是可选、可插拔的。

Kubernetes 为构建开发人员平台提供了基础，但是在重要的地方保留了用户选择权，能有更高的灵活性。

- 不限制支持的应用程序类型。如果应用程序可以在容器中运行，那么它应该可以在 Kubernetes 上很好地运行。

- 不部署源代码，也不构建你的应用程序。

  持续集成（CI）、交付和部署（CI/CD）工作流取决于组织的文化和偏好以及技术要求。

- 不提供应用程序级别的服务作为内置服务，例如中间件（例如消息中间件）、 数据处理框架（例如 Spark）、数据库（例如 MySQL）、缓存、集群存储系统 （例如 Ceph）。这样的组件可以在 Kubernetes 上运行，并且/或者可以由运行在 Kubernetes 上的应用程序通过可移植机制（例如[开放服务代理](https://openservicebrokerapi.org/)）来访问。

- 不是日志记录、监视或警报的解决方案。 它集成了一些功能作为概念证明，并提供了收集和导出指标的机制。

- 不提供也不要求配置用的语言、系统（例如 jsonnet），它提供了声明性 API， 该声明性 API 可以由任意形式的声明性规范所构成。

- 不提供也不采用任何全面的机器配置、维护、管理或自我修复系统。

- 此外，Kubernetes 不仅仅是一个编排系统，实际上它消除了编排的需要。 

  编排的技术定义是执行已定义的工作流程：首先执行 A，然后执行 B，再执行 C。 而 Kubernetes 包含了一组独立可组合的控制过程，可以持续地将当前状态驱动到所提供的预期状态。 你不需要在乎如何从 A 移动到 C，也不需要集中控制，这使得系统更易于使用且功能更强大、 系统更健壮，更为弹性和可扩展。



## 容器优势

容器因具有许多优势而变得流行起来，例如：

- 敏捷应用程序的创建和部署：与使用 VM 镜像相比，提高了容器镜像创建的简便性和效率。
- 持续开发、集成和部署：通过快速简单的回滚（由于镜像不可变性）， 提供可靠且频繁的容器镜像构建和部署。
- 关注开发与运维的分离：在构建、发布时创建应用程序容器镜像，而不是在部署时， 从而将应用程序与基础架构分离。
- 可观察性：不仅可以显示 OS 级别的信息和指标，还可以显示应用程序的运行状况和其他指标信号。

- 跨开发、测试和生产的环境一致性：在笔记本计算机上也可以和在云中运行一样的应用程序。
- 跨云和操作系统发行版本的可移植性：可在 Ubuntu、RHEL、CoreOS、本地、 Google Kubernetes Engine 和其他任何地方运行。
- 以应用程序为中心的管理：提高抽象级别，从在虚拟硬件上运行 OS 到使用逻辑资源在 OS 上运行应用程序。
- 松散耦合、分布式、弹性、解放的微服务：应用程序被分解成较小的独立部分， 并且可以动态部署和管理 - 而不是在一台大型单机上整体运行。
- 资源隔离：可预测的应用程序性能。
- 资源利用：高效率和高密度。



# Kubernetes 组件

正常运行的 K8S 集群所需的各种组件。

![](img/components-of-kubernetes.svg)**核心组件**

K8S 集群由 **控制平面** 和 **一个或多个工作节点**组成。



## 控制平面组件（Control Plane Components）

管理集群的整体状态：

- **kube-apiserver**

  公开 Kubernetes HTTP API 的核心组件服务器，上图右侧 **API server**

- **etcd**

  具备一致性和高可用性的键值存储，用于所有 API 服务器的数据存储，上图右侧 **etcd**

- **kube-scheduler**

  查找尚未绑定到节点的 Pod，并将每个 Pod 分配给合适的节点。上图右侧 **Scheduler**

- **kube-controller-manager**

  运行控制器来实现 Kubernetes API 行为。上图右侧 **Controller manager**

- **cloud-controller-manager**  (optional)

  与底层云驱动集成



## Node 组件

在每个节点上运行，维护运行的 Pod 并提供 Kubernetes 运行时环境：

- **kubelet**

  确保 Pod 及其容器正常运行。

- **kube-proxy**（可选）

  维护节点上的网络规则以实现 Service 的功能。

- **容器运行时（Container runtime）**

  负责运行容器的软件。

你的集群可能需要每个节点上运行额外的软件；例如，你可能还需要在 Linux 节点上运行 [systemd](https://systemd.io/) 来监督本地组件。



## 插件（Addons）

插件扩展了 Kubernetes 的功能。一些重要的例子包括：

- **DNS**

  集群范围内的 DNS 解析

- **Web 界面**

  通过 Web 界面进行集群管理

- 容器资源监控

  用于收集和存储容器指标

- 集群层面日志

  用于将容器日志保存到中央日志存储



## 架构灵活性

Kubernetes 允许灵活地部署和管理这些组件。此架构可以适应各种需求，从小型开发环境到大规模生产部署。





# Kubernates 对象

Kubernetes 对象是 Kubernetes 系统中的**持久性实体**。 Kubernetes 使用这些实体表示你的**集群状态**。 

这部分说明了在 Kubernetes API 中是如何表示 Kubernetes 对象的， 以及如何使用 `.yaml` 格式的文件表示 Kubernetes 对象。



**理解 Kubernetes 对象**

在 Kubernetes 系统中，**Kubernetes 对象**是持久化的实体。 Kubernetes 使用这些实体去表示整个集群的状态。 

具体而言，它们描述了如下信息：

- 哪些容器化应用正在运行（以及在哪些节点上运行）
- 可以被应用使用的资源
- 关于应用运行时行为的策略，比如重启策略、升级策略以及容错策略

Kubernetes 对象是一种“意向表达（Record of Intent）”。一旦创建该对象， Kubernetes 系统将不断工作以确保该对象存在。通过创建对象，你本质上是在告知 Kubernetes 系统，你想要的集群工作负载状态看起来应是什么样子的， 这就是 Kubernetes 集群所谓的**期望状态（Desired State）**。

操作 Kubernetes 对象 —— 无论是创建、修改或者删除 —— 需要使用 Kubernetes API。 比如，当使用 kubectl 命令行接口（CLI）时，CLI 会调用必要的 Kubernetes API； 也可以在程序中使用客户端库， 来直接调用 Kubernetes API。



**对象规约（Spec）与状态（Status）**

几乎每个 Kubernetes 对象包含两个嵌套的对象字段，它们负责管理对象的配置： 对象 **`spec`（规约）** 和对象 **`status`（状态）**。

对于具有 `spec` 的对象，你必须在创建对象时设置其内容，描述你希望对象所具有的特征： **期望状态（Desired State）**。

status 描述了对象的当前状态（Current State），它是由 Kubernetes 系统和组件设置并更新的。在任何时刻，Kubernetes 控制平面 都一直在积极地管理着对象的实际状态，以使之达成期望状态。

例如，Kubernetes 中的 Deployment 对象能够表示运行在集群中的应用。 当创建 Deployment 时，你可能会设置 Deployment 的 `spec`，指定该应用要有 3 个副本运行。 Kubernetes 系统读取 Deployment 的 `spec`， 并启动我们所期望的应用的 3 个实例 —— 更新状态以与规约相匹配。 如果这些实例中有的失败了（一种状态变更），Kubernetes 系统会通过执行修正操作来响应 `spec` 和 `status` 间的不一致 —— 意味着它会启动一个新的实例来替换。



**描述 Kubernetes 对象**

创建 Kubernetes 对象时，必须提供对象的 `spec`，用来描述该对象的期望状态， 以及关于对象的一些基本信息（例如名称）。 当使用 Kubernetes API 创建对象时（直接创建或经由 `kubectl` 创建）， API 请求必须在请求主体中包含 JSON 格式的信息。 大多数情况下，你会通过 **清单（Manifest）** 文件为 `kubectl` 提供这些信息。 

按照惯例，清单是 YAML 格式的（你也可以使用 JSON 格式）。 像 `kubectl` 这样的工具在通过 HTTP 进行 API 请求时， 会将清单中的信息转换为 JSON 或其他受支持的序列化格式。

这里有一个清单示例文件，展示了 Kubernetes Deployment 的必需字段和对象 `spec`：

~~~yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # 告知 Deployment 运行 2 个与该模板匹配的 Pod
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
~~~

与上面使用清单文件来创建 Deployment 类似，另一种方式是使用 kubectl 命令行接口（CLI）的 kubectl apply 命令， 将 .yaml 文件作为参数。下面是一个示例：

~~~shell
kubectl apply -f https://k8s.io/examples/application/deployment.yaml
~~~

请求远程 deployment.yaml

输出类似下面这样：

~~~shell
deployment.apps/nginx-deployment created
~~~



**必需字段**

在想要创建的 Kubernetes 对象所对应的清单（YAML 或 JSON 文件）中，需要配置的字段如下：

- `apiVersion` - 创建该对象所使用的 Kubernetes API 的版本
- `kind` - 想要创建的对象的类别
- `metadata` - 帮助唯一标识对象的一些数据，包括一个 `name` 字符串、`UID` 和可选的 `namespace`
- `spec` - 你所期望的该对象的状态

对每个 Kubernetes 对象而言，其 spec 之精确格式都是不同的，包含了特定于该对象的嵌套字段。 Kubernetes API 参考可以帮助你找到想要使用 Kubernetes 创建的所有对象的规约格式。

例如，参阅 Pod API 参考文档中 [`spec` 字段](https://kubernetes.io/zh-cn/docs/reference/kubernetes-api/workload-resources/pod-v1/#PodSpec)。 对于每个 Pod，其 `.spec` 字段设置了 Pod 及其期望状态（例如 Pod 中每个容器的容器镜像名称）。 另一个对象规约的例子是 StatefulSet API 中的 [`spec` 字段](https://kubernetes.io/zh-cn/docs/reference/kubernetes-api/workload-resources/stateful-set-v1/#StatefulSetSpec)。 对于 StatefulSet 而言，其 `.spec` 字段设置了 StatefulSet 及其期望状态。 在 StatefulSet 的 `.spec` 内，有一个为 Pod 对象提供的[模板](https://kubernetes.io/zh-cn/docs/concepts/workloads/pods/#pod-templates)。 该模板描述了 StatefulSet 控制器为了满足 StatefulSet 规约而要创建的 Pod。 不同类型的对象可以有不同的 `.status` 信息。API 参考页面给出了 `.status` 字段的详细结构， 以及针对不同类型 API 对象的具体内容。



**服务器端字段验证**

从 Kubernetes v1.25 开始，API 服务器提供了服务器端字段验证， 可以检测对象中未被识别或重复的字段。它在服务器端提供了 `kubectl --validate` 的所有功能。

`kubectl` 工具使用 `--validate` 标志来设置字段验证级别。它接受值 `ignore`、`warn` 和 `strict`，同时还接受值 `true`（等同于 `strict`）和 `false`（等同于 `ignore`）。`kubectl` 的默认验证设置为 `--validate=true`。

- `Strict`

  严格的字段验证，验证失败时会报错

- `Warn`

  执行字段验证，但错误会以警告形式提供而不是拒绝请求

- `Ignore`

  不执行服务器端字段验证

当 `kubectl` 无法连接到支持字段验证的 API 服务器时，它将回退为使用客户端验证。 Kubernetes 1.27 及更高版本始终提供字段验证；较早的 Kubernetes 版本可能没有此功能。 如果你的集群版本低于 v1.27，可以查阅适用于你的 Kubernetes 版本的文档。



## Kubernates 对象管理

`kubectl` 命令行工具支持多种不同的方式来创建和管理 Kubernetes 对象。 

| 管理技术       | 作用于   | 建议的环境 | 支持的写者 | 学习难度 |
| -------------- | -------- | ---------- | ---------- | -------- |
| 指令式命令     | 活跃对象 | 开发项目   | 1+         | 最低     |
| 指令式对象配置 | 单个文件 | 生产项目   | 1          | 中等     |
| 声明式对象配置 | 文件目录 | 生产项目   | 1+         | 最高     |

---

**指令式命令**

> 创建 deployment 对象

~~~shell
$ kubectl create deployment nginx --image nginx
~~~

**权衡**

与对象配置相比的优点：

- 命令用单个动词表示。
- 命令仅需一步即可对集群进行更改。

与对象配置相比的缺点：

- 命令不与变更审查流程集成。
- 命令不提供与更改关联的审核跟踪。
- 除了实时内容外，命令不提供记录源。
- 命令不提供用于创建新对象的模板。

---

**指令式对象配置**

创建配置文件中定义的对象：

~~~shell
$ kubectl create -f nginx.yaml
~~~

删除两个配置文件中定义的对象：

~~~shell
$ kubectl delete -f nginx.yaml -f redis.yaml
~~~

通过覆盖活动配置来更新配置文件中定义的对象：

~~~shell
$ kubectl replace -f nginx.yaml
~~~

**权衡**

与指令式命令相比的优点：

- 对象配置可以存储在源控制系统中，比如 Git。
- 对象配置可以与流程集成，例如在推送和审计之前检查更新。
- 对象配置提供了用于创建新对象的模板。

与指令式命令相比的缺点：

- 对象配置需要对对象架构有基本的了解。
- 对象配置需要额外的步骤来编写 YAML 文件。

与声明式对象配置相比的优点：

- 指令式对象配置行为更加简单易懂。
- 从 Kubernetes 1.5 版本开始，指令对象配置更加成熟。

与声明式对象配置相比的缺点：

- 指令式对象配置更适合文件，而非目录。
- 对活动对象的更新必须反映在配置文件中，否则会在下一次替换时丢失。

---

**声明式对象配置**

处理 `configs` 目录中的所有对象配置文件，创建并更新活跃对象。 可以首先使用 `diff` 子命令查看将要进行的更改，然后在进行应用：

~~~shell
$ kubectl diff -f configs/

$ kubectl apply -f configs/
~~~

递归处理目录：

~~~shell
$ kubectl diff -R -f configs/

$ kubectl apply -R -f configs/
~~~

**权衡**

与指令式对象配置相比的优点：

- 对活动对象所做的更改即使未合并到配置文件中，也会被保留下来。
- 声明性对象配置更好地支持对目录进行操作并自动检测每个文件的操作类型（创建，修补，删除）。

与指令式对象配置相比的缺点：

- 声明式对象配置难于调试并且出现异常时结果难以理解。
- 使用 diff 产生的部分更新会创建复杂的合并和补丁操作。

---





## 对象名称和 ID

集群中的每一个对象都有一个名称来标识在同类资源中的唯一性。

每个 Kubernetes 对象也有一个 UID 来标识在整个集群中的唯一性。

比如，在同一个**名字空间**中只能有一个名为 `myapp-1234` 的 Pod，但是可以命名一个 Pod 和一个 Deployment 同为 `myapp-1234`。

对于用户提供的非唯一性的属性，Kubernetes 提供了**标签（Label）**和 **注解（Annotation）**机制。

**对象名称**

> ```yaml
> metadata:
>   name: nginx-demo
> ```



路径分段名称：

某些资源类型要求名称能被安全地用作路径中的片段。 换句话说，其名称不能是 `.`、`..`，也不可以包含 `/` 或 `%` 这些字符。

下面是一个名为 `nginx-demo` 的 Pod 的配置清单：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-demo
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```



**UID**

Kubernetes 系统生成的字符串，唯一标识对象。

> uid: 0a48432a-7ec9-4001-898d-81f9d1358fa9

~~~shell
[node2 ~]$ kubectl get -f https://k8s.io/examples/application/simple_deployment.yaml -o yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"nginx-deployment","namespace":"default"},"spec":{"minReadySeconds":5,"selector":{"matchLabels":{"app":"nginx"}},"template":{"metadata":{"labels":{"app":"nginx"}},"spec":{"containers":[{"image":"nginx:1.14.2","name":"nginx","ports":[{"containerPort":80}]}]}}}}
  creationTimestamp: "2025-02-12T09:00:17Z"
  generation: 1
  name: nginx-deployment
  namespace: default
  resourceVersion: "1001"
  uid: 0a48432a-7ec9-4001-898d-81f9d1358fa9
spec:
  minReadySeconds: 5
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: nginx
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:1.14.2
        imagePullPolicy: IfNotPresent
        name: nginx
        ports:
        - containerPort: 80
          protocol: TCP
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2025-02-12T09:00:30Z"
    lastUpdateTime: "2025-02-12T09:00:30Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2025-02-12T09:00:17Z"
    lastUpdateTime: "2025-02-12T09:00:30Z"
    message: ReplicaSet "nginx-deployment-cbdccf466" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 1
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1
~~~



## Lables 和 Selectors

> 标签和选择算符
>
> https://kubernetes.io/zh-cn/docs/concepts/overview/working-with-objects/labels/





























