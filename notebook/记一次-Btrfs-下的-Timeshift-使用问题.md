# 记一次Btrfs下的Timeshift使用问题

问题描述：Btrfs下，使用Timeshift恢复快照时，无法挂载home,报错类似

```shell
[FAILED] Failed to mount /home.
[DEPEND] Dependency failed for Local File Systems.
```

解决方案：详细可见[从timeshift restore导致home挂载失败说起](https://blog.dreamfever.me/2021/12/08/cong-timeshift-restore-dao-zhi-home-gua-zai-shi-bai-shuo-qi/)

```shell
btrfs subvolume --list /  # 查看btrfs子卷
修改/etc/fstab下的子卷subvolid
```