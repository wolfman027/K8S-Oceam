**Deployment** 是一种控制器对象，用于管理 Pods 的创建和更新过程。它提供了一种声明式的方法来描述期望的应用状态。

**Pod** 是 Kubernetes 中最小的可部署计算单元。





# Node

## 获取集群节点

~~~shell
$ kubectl get nodes
~~~





# deployment

## 获取详情

~~~shell
[node2 ~]$ kubectl get -f https://k8s.io/examples/application/simple_deployment.yaml -o yaml
~~~



## 删除

> **直接删除**
>
> kubectl delete deployment <deployment-name>
>
> 例如：`kubectl delete deployment hello-minikube`
>
> **在命令空间中删除 deployment**
>
> kubectl delete deployment <deployment-name> --namespace <namespace-name>
>
> 例如：kubectl delete deployment nginx-deployment --namespace qa



~~~shell
$ kubectl delete deployment hello-minikube
deployment.apps "hello-minikube" deleted
~~~





# Pod

### 根据 yaml 创建 Pod

~~~shell
$ kubectl create -f pod.yaml
~~~







### 获取所有的 pod

~~~shell
$ kubectl get pods
~~~



## 获取 pod 日志

> kubectl logs hello-pt4j2 -c test
>
> 如果 pod 中有多个 container，则需要增加 `-c test` 后边test 是容器的名称

~~~shell
h.a.hu@AMAR64XLRH7PP ~ $ kubectl logs hello-pt4j2
Hello, Kubernetes!


h.a.hu@AMAR64XLRH7PP ~ $ kubectl logs hello-pt4j2 -c test
日志信息
~~~



### 查看 pod 容器的状态

~~~shell
$ kubectl describe pod nginx
~~~







## 基于等值的标签

~~~shell
kubectl get pods -l environment=production,tier=frontend
~~~



## 基于集合需求

```shell
kubectl get pods -l 'environment in (production),tier in (frontend)'
```





# Service

~~~shell
$ kubectl get svc
~~~



















