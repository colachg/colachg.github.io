+++ 
draft = false
date = 2019-02-27T16:28:33+08:00
title = "Make read-only kubeconfig"
slug = "/rbac" 
tags = []
categories = []
+++

# 基本概念

## Role and ClusterRole

- Role 是一组权限的集合。权限的机制纯粹是添加的，没有禁止的规则。Role 是在命名空间里定义的,集群中的角色只能被赋予该集群某些资源的访问权限。下面是一个在 default 命名空间下，对 pods 只读权限的例子:

```
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  # "namespace" omitted since ClusterRoles are not namespaced
  name: secret-reader
rules:
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get", "watch", "list"]
```

- 集群级别的 Role 就是 ClusterRole，它的权限范围就是整个集群级别的。下面就是可以在任何命名空间下读取 secrets 的 ClusterRole:

```
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  # "namespace" omitted since ClusterRoles are not namespaced
  name: secret-reader
rules:
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get", "watch", "list"]
```

## RoleBinding and ClusterRoleBinding

- RoleBinding 是用来绑定 role 和用户的，它的范围是在一个命名空间里面。ClusterRoleBinding 类似，只不过是集群级别的。rolebinding 的 subject 可以是(users,groups 或 service accounts)：

```
# This role binding allows "jane" to read pods in the "default" namespace.
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: read-pods
  namespace: default
subjects:
- kind: User
  name: jane # Name is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role #this must be Role or ClusterRole
  name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
  apiGroup: rbac.authorization.k8s.io
```

- 一个集群中可一有多个命名空间，一个 RoleBinding 也可以绑定 ClusterRole，这样可以给管理员带来便利。

# 创建只读权限的 kubeconfig 文件(readonly)

- step1: 创建一个 service account 在 kube-system 下，可使用如下[脚本](https://gist.github.com/colachg/6c5c6e78477e2c7c9b6c130507c1e532)  
   `kubernetes_add_service_account_kubeconfig.sh view kube-system`
- step2: 将系统自带的 clusterrole:view 通过 clusterrolebinding 绑定 service account system:view.
  `kubectl create clusterrolebinding system:view --clusterrole=view --serviceaccount=kube-system:view`
- step3: 将`/tmp/kube/k8s-${SERVICE_ACCOUNT_NAME}-${NAMESPACE}-conf`拷贝至`~/.kube/config`测试结果.

## reference

https://kubernetes.io/docs/reference/access-authn-authz/rbac/  
https://docs.bitnami.com/kubernetes/how-to/configure-rbac-in-your-kubernetes-cluster/#step-2-create-the-user-credentials
