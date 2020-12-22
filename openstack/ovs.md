# 虚拟机与ovs port 关系查看



查看VM的interface 于 ovs bridge 的port对应关系：

```shell
ovs-appctl fdb/show br-int
```

查看虚拟机绑定的port信息：

```
virsh domiflist <instance-id>
```

![image-20201217165353971](ovs.assets/image-20201217165353971.png)