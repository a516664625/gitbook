### 1. 支持revert 任意snapshot

1.将**0001-cinder-revert-snapshot-support-any-snapshot.patch** 文件复制到**cinder_api**与**cinder_volume**容器中

```sehll
docker cp 0001-cinder-revert-snapshot-support-any-snapshot.patch cinder_api:/
docker cp 0001-cinder-revert-snapshot-support-any-snapshot.patch cinder_volume:/
```

2.进入cinder_api容器,应用补丁

```shell
docker exec -it cinder_api bash
cd /var/lib/kolla/venv/lib/python2.7/site-packages/
git apply <patch_path>
```

3.进入cinder_volume容器,应用补丁

```shell
docker exec -it cinder_volume bash
cd /var/lib/kolla/venv/lib/python2.7/site-packages/
git apply <patch_path>
```

### 2.rbd后端直接revert snapshot

1.将**0001-RBD-add-support-for-revert-to-snapshot.patch**文件复制到cinder_volume容器中

```shell
docker cp 0001-cinder-revert-snapshot-support-any-snapshot.patch cinder_volume:/
```

2.进入cinder_volume容器,应用补丁

```shell
docker exec -u root -it cinder_volume bash 
cd /var/lib/kolla/venv/lib/python2.7/site-packages/
git apply <patch_path>
```

### 3.判断volume是否真的在后端存储上使用

1.将**0001-when-the-volume-in-use-in-backend-raise-error.patch**文件复制到cinder_volume中

```shell
docker cp 0001-cinder-revert-snapshot-support-any-snapshot.patch cinder_volume:/
```

2.进入cinder_volume容器,应用补丁

```shell
docker exec -u root -it cinder_volume bash 
cd /var/lib/kolla/venv/lib/python2.7/site-packages/
git apply <patch_path>
```

### 更新完成后重启cinder_volume,cinder_api 容器

