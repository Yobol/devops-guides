# kubeconfig

`K8S` 使用 `kubeconfig` 文件（用于配置 `K8S` 集群访问的文件**统称**）来组织集群、用户、命名空间和身份认证机制的信息。

```yaml
apiVersion: v1
kind: Config
clusters:
- cluster:
    certificate-authority-data: <base64_ca>
    server: https://12.34.56.78:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: <base64_ca>
    client-key-data: <base64_pem>
```

注：

- 查看证书：`echo "<base64_ca> | base64 -d | openssl x509 -text -noout -in -"`
- 查看证书公钥：`echo "<base64_ca> | base64 -d | openssl x509 -pubkey -noout -in -"`
- 查看私钥：`echo "<base64_pem> | base64 -d | openssl rsa -text -noout -in -"`

`kubectl` 命令行工具使用 `kubeconfig` 文件来查找选择集群所需的信息，并于集群的 `api-server` 进行通信。**默认情况下，`kubectl` 使用 `$HOME/.kube/config` 文件，可以使用 `KUBECONFIG` 环境变量或设置 `--kubeconfig` 参数来指定其他文件。**

```shell
export KUBECONFIG=${HOME}/.kube/config && kubectl get nodes
# 等价于
kubectl --kubeconfig=${HOME}/.kube/config get nodes
```

某些情况下，当前用户目录下并不存在 `.kube/config` 文件，可以通过如下操作拷贝：

```shell
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

需要特别注意的是：`kubeconfig` 文件中的文件和路径引用是相对于 `kubeconfig` 文件的位置，而命令行上的文件和目录引用是相对于当前工作目录的。当不确定上述区别时，更保险的做法是使用绝对路径。

## 集群

### 使用代理

可以在 `kubeconfig` 文件中为指定集群配置 `proxy-url` 来让 `kubectl` 使用代理：

```yaml
apiVersion: v1
kind: Config

clusters:
- cluster:
    proxy-url: http://proxy.example.org:3128
    server: https://k8s.example.org/k8s/clusters/xyz
  name: development

users:
- user:
  name: developer

contexts:
- context:
    user: developer
    cluster: development
  name: developer@development
 
```

## 用户

## 上下文

## 命令

`kubeconfig` 中的配置可以使用 `kubectl config` 命令进行管理，使用时通过 `kubectl config --help` 可以查看帮助。

### 合并 kubeconfig

使用 `kubectl config view` 命令可以查看 `kubeconfig` 文件的合并结果。

`kubectl` 在合并 `kubeconfig` 文件时使用如下规则：

- 如果设置了 `--kubeconfig` 参数（只能使用一次），则仅使用指定的文件，不做合并；
- 否则，如果设置了 `KUBECONFIG` 环境变量，则将它作为应合并的文件列表，如 `export KUBECONFIG={KUBECONFIG}:config-demo-1:config-demo-2` ；
- 否则，使用默认的 `kubeconfig` 文件 `$HOME/.kube/config`，不做合并。

### 查看及切换当前上下文

默认情况下，`kubectl` 命令使用当前上下文中的参数与集群进行通信，可以使用 `kubectl config use-context` 切换当前上下文。

```shell
# 查看所有上下文
kubectl config get-contexts
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   
          developer@development         development  developer

# 列出当前上下文
kubectl config current-context
kubernetes-admin@kubernetes

# 切换当前上下文
kubectl config use-context developer@development
Switched to context "developer@development".
```

## 参考

1. [使用 kubeconfig 文件组织集群访问](https://kubernetes.io/zh-cn/docs/concepts/configuration/organize-cluster-access-kubeconfig/)
2. [配置对多集群的使用](https://kubernetes.io/zh-cn/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)
