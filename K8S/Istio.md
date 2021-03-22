# Istio

官方文档：https://istio.io/latest/zh/docs/





查看Pod （productpage-v1-745ffc55b7-2l2lw）中运行的容器：

```shell
kubectl -n default get pod productpage-v1-745ffc55b7-2l2lw -o=jsonpath='{..spec.containers[*].name}'
productpage istio-proxy
```

istio-proxy 即Envoy 代理的sidecar容器。另外该Pod中实际上还运行过一个Init容器，因为它执行结束后自动终止了，所以我们看不到该容器的存在。









