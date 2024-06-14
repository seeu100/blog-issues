Arch4edu 是面向高校用户推出的非官方软件仓库，支持 Arch Linux 和 Arch Linux ARM，主要包含高校用户常用的科研、教学及开发软件。

## 使用方法

1.  导入 GPG key
```shell
pacman-key --recv-keys 7931B6D628C8D3BA
pacman-key --finger 7931B6D628C8D3BA
pacman-key --lsign-key 7931B6D628C8D3BA
```
2.    在 `/etc/pacman.conf` 文件末尾添加以下内容：

```shell
[arch4edu]
Server = https://mirrors.tuna.tsinghua.edu.cn/arch4edu/$arch
```