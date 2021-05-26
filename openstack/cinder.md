## cinder  revert-to-snapshot

1.创建云主机A（系统盘+数据盘）
2.创建快照1（系统盘快照1+数据盘快照1）
3.创建快照2（系统盘快照2+数据盘快照2）
4.创建快照3（系统盘快照3+数据盘快照3）
5.云主机A关机，回滚快照1或者2或者3
6.云主机A开机

```python
cinder api/v3/volumes  可以revert任意的snapshot
cinder /volume/api     可以revert in-use的snapshot
cinder /volume/mananger  修改状态
cinder /volume/driver 支持在后端直接revert snapshot
```

```shell
nova boot --flavor test --nic net-id=f8b27685-377f-47ed-858e-6228449a8a37 --block-device source=image,id=c77b9328-d2da-41c6-b2f4-d947e0f4c0e1,dest=volume,size=1,bootindex=0 boot_from_volume
```

cinder 支持 revert 扩展后的硬盘



