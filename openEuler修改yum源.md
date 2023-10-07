# openEuler修改yum源

## 备份

```bash




cd /etc/yum.repos.d



mkdir backup



mv *.repo backup/
```

## 添加源配置

```bash
dnf config-manager --add-repo=https://mirrors.aliyun.com/openeuler/openEuler-20.03-LTS/OS/x86_64/
dnf config-manager --add-repo=https://mirrors.aliyun.com/openeuler/openEuler-20.03-LTS/everything/x86_64/
```

## 构建缓存

```bash
yum makecache
```

