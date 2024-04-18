接上篇 #15  [archlinux 安装](https://blog.00002000.xyz/post/15.html) 

### 配置镜像
```shell
echo 'Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch' > /etc/pacman.d/mirrorlist
```
### 配置缓存策略，仅保存当前包版本
```shell
sed -i -e "s/^#CleanMethod = KeepInstalled/CleanMethod = KeepCurrent/" /etc/pacman.conf
```
### 初始化 keyring，这是关键安全配置

https://wiki.archlinux.org/title/Pacman/Package_signing

```shell
pacman-key --init && pacman-key --populate
pacman -Sy archlinux-keyring && pacman -Su
```
## Arch Linux CN / Bioarchlinux 源
```shell
sudo vim /etc/pacman.conf 
# 添加下列内容
[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch

[bioarchlinux]
# China
# ## ISCAS
Server = https://mirror.iscas.ac.cn/BioArchLinux/$arch
# ## Nanjing University
Server = https://mirror.nju.edu.cn/bioarchlinux/$arch
# ## Nanjing University of Posts and Telecommunications
# Server = http://mirrors.njupt.edu.cn/bioarchlinux/$arch
Server = https://mirrors.njupt.edu.cn/bioarchlinux/$arch
# ## Shandong University
# Server = http://mirrors.sdu.edu.cn/bioarchlinux/$arch
Server = https://mirrors.sdu.edu.cn/bioarchlinux/$arch
Server = https://repo.bioarchlinux.org/$arch

# 手动信任bioarchlinux key
sudo pacman-key --recv-keys B1F96021DB62254D
sudo pacman-key --finger B1F96021DB62254D
sudo pacman-key --lsign-key B1F96021DB62254D

# 手动信任archlinuxcn key (临时)
sudo pacman-key --lsign-key "farseerfc@archlinux.org"

# 安装keyring包
sudo pacman -Sy && sudo pacman -S archlinuxcn-keyring  bioarchlinux-keyring && sudo pacman -S paru #AUR安装器
```

<!-- ##{"timestamp":1682524800}## -->