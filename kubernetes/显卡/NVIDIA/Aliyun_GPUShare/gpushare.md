# Aliyun Container Service - gpushare

> install cuda (latest: cuda, specific: cuda-11-8, cuda-12-0, ...)
>
> install nvidia-docker2 && config docker's default-runtime as nvidia
>
> Kubernetes v1.23+

```shell
cd /etc/kubernetes
sudo curl -O https://raw.githubusercontent.com/AliyunContainerService/gpushare-scheduler-extender/master/config/scheduler-policy-config.yaml
sudo vim scheduler-policy-config.yaml
# 默认的 apiVersion 可能是 kubescheduler.config.k8s.io/v1beta2，需要修改为如下配置
# apiVersion: kubescheduler.config.k8s.io/v1

sudo vim manifests/kube-scheduler.yaml
# [add container command argument]
# - --config=/etc/kubernetes/scheduler-policy-config.yaml
# [add container volumeMount]
# - mountPath: /etc/kubernetes/scheduler-policy-config.yaml
#   name: scheduler-policy-config
#   readOnly: true
# [add volume]
# - hostPath:
#     path: /etc/kubernetes/scheduler-policy-config.yaml
#     type: FileOrCreate
#   name: scheduler-policy-config

cd -
mkdir gpushare-scheduler-extender && gpushare-scheduler-extender

curl -O https://raw.githubusercontent.com/AliyunContainerService/gpushare-scheduler-extender/master/config/gpushare-schd-extender.yaml
# 如果想在 master 上可以进行调度，可以将 nodeSelector: node-role.kubernetes.io/master: "" 删除
kubectl create -f gpushare-schd-extender.yaml

curl -O https://raw.githubusercontent.com/AliyunContainerService/gpushare-device-plugin/master/device-plugin-rbac.yaml
kubectl create -f device-plugin-rbac.yaml

curl -O https://raw.githubusercontent.com/AliyunContainerService/gpushare-device-plugin/master/device-plugin-ds.yaml
# 默认情况下，GPU 显存以 GiB 为单位，若需要使用 MiB 为单位，需要在这个文件中，将 --memory-unit=GiB 修改为 --memory-unit=MiB
kubectl create -f device-plugin-ds.yaml

# 选取 GPU 节点打上标签
kubectl get nodes
kubectl label node <node> gpushare=true

# 安装 kubectl-inspect-gpushare 工具
wget https://github.com/AliyunContainerService/gpushare-device-plugin/releases/download/v0.3.0/kubectl-inspect-gpushare
chmod +x kubectl-inspect-gpushare
sudo mv kubectl-inspect-gpushare /usr/local/bin
# 使用 kubectl-inspect-gpushare 工具
kubectl inspect gpushare -d

```

注：`/etc/kubernetes/manifests/kube-scheduler.yaml` 可以参考 [kube-scheduler.yaml](https://github.com/AliyunContainerService/gpushare-scheduler-extender/blob/master/config/kube-scheduler-v1.23+.yaml)。


上述步骤完成后，可以通过 [官方示例](https://github.com/AliyunContainerService/gpushare-scheduler-extender/tree/master/samples) 进行测试。

## 参考

1. [gpushare install guide](https://github.com/AliyunContainerService/gpushare-scheduler-extender/blob/master/docs/install.md)
