# 本地存储

1. [single-sc-on-single-disk](https://github.com/Yobol/containered-deployments/tree/main/helm-chart/pv-provisioner/host-path/local-path-provisioner/v0.0.25/single-sc-on-single-disk)：一般在本地只有一种存储介质时使用，如 `hdd`、`nvme`、`sata` 只有其一；
2. [multiple-sc-on-different-disks](https://github.com/Yobol/containered-deployments/tree/main/helm-chart/pv-provisioner/host-path/local-path-provisioner/v0.0.25/multiple-sc-on-difference-disks)：一般在本地有多种存储介质时使用，如 `hdd`、`nvme`、`sata` 混合使用；

注：

- 安装 `kustomize` 工具：`GOBIN=$(pwd)/ GO111MODULE=on go install sigs.k8s.io/kustomize/kustomize/v5@latest`；
- 设置默认 `storageclass`：`kubectl patch storageclass local-path -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`。
