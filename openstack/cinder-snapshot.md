### cinder snapshot 优化

##### 1.cinder revert 

- cinder train 版本的在ceph做后端时volume revert snapshot实际上是从snapshot创建一块盘然后将数据复制到 要revert的volume中




```python
_create_temp_volume_from_snapshot ->_copy_volume_data->delete_temp_volume
```

在社区新的版本中实现了在用ceph后端时 volume revert snapshot 直接从后端存储rollback

patch:https://review.opendev.org/c/openstack/cinder/+/710334

##### 2.cinder create from volume/snapshot

当cinder 从snapshot或者从volume中创建卷后因为依赖关系不能删除原有的卷或快照,可以在配置项打开这两个配置项来避免

```shell
[rbd-1]
rbd_flatten_volume_from_snapshot = true
rbd_max_clone_depth = 0
```

Librbd 在卷的克隆时会形成子卷对父卷的依赖，在产生较长的克隆依赖链后会有严重的性能损耗”。这个理论其实和cow下多快照产生的性能衰减是一样的，对ceph的云硬盘做快照，每次做完快照后再对云硬盘进行写入时就会触发COW操作， 即1次读操作、2次写操作，volume→volume的克隆本质上就是将 volume 的某一个 Snapshot 的状态复制变成另一个volume。

rbd_flatten_volume_from_snapshot 配置为true让 从sanpshot 创建的volume不再有依赖 rbd_max_clone_depth 这个参数控制卷克隆的最大层数，超过的话则使用 fallten。设为 0 的话，则禁止克隆。