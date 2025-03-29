接上篇 #15  [archlinux 安装](https://blog.00002000.xyz/post/15.html) 

### 配置`/etc/pacman.conf`
```shell
cat << EOF > /etc/pacman.conf
#
# /etc/pacman.conf
#
# See the pacman.conf(5) manpage for option and repository directives

#
# GENERAL OPTIONS
#
[options]
# The following paths are commented out with their default values listed.
# If you wish to use different paths, uncomment and update the paths.
#RootDir     = /
#DBPath      = /var/lib/pacman/
#CacheDir    = /var/cache/pacman/pkg/
#LogFile     = /var/log/pacman.log
#GPGDir      = /etc/pacman.d/gnupg/
#HookDir     = /etc/pacman.d/hooks/
HoldPkg     = pacman glibc
#XferCommand = /usr/bin/curl -L -C - -f -o %o %u
#XferCommand = /usr/bin/wget --passive-ftp -c -O %o %u
#XferCommand = /bin/aria2c -c --max-tries=2 --max-connection-per-server=2 --max-file-not-found=3 --remote-time=true --timeout=3 -d / -o %o %u
#CleanMethod = KeepInstalled
Architecture = auto

# Pacman won't upgrade packages listed in IgnorePkg and members of IgnoreGroup
#IgnorePkg   =
#IgnoreGroup =

#NoUpgrade   =
#NoExtract   =

# Misc options
#UseSyslog
Color
ILoveCandy
#NoProgressBar
CheckSpace
#VerbosePkgLists
ParallelDownloads = 3
DownloadUser = alpm
#DisableSandbox

# By default, pacman accepts packages signed by keys that its local keyring
# trusts (see pacman-key and its man page), as well as unsigned packages.
SigLevel    = Required DatabaseOptional
LocalFileSigLevel = Optional
#RemoteFileSigLevel = Required

# NOTE: You must run `pacman-key --init` before first using pacman; the local
# keyring can then be populated with the keys of all official Arch Linux
# packagers with `pacman-key --populate archlinux`.

#
# REPOSITORIES
#   - can be defined here or included from another file
#   - pacman will search repositories in the order defined here
#   - local/custom mirrors can be added here or in separate files
#   - repositories listed first will take precedence when packages
#     have identical names, regardless of version number
#   - URLs will have $repo replaced by the name of the current repo
#   - URLs will have $arch replaced by the name of the architecture
#
# Repository entries are of the format:
#       [repo-name]
#       Server = ServerName
#       Include = IncludePath
#
# The header [repo-name] is crucial - it must be present and
# uncommented to enable the repo.
#

# The testing repositories are disabled by default. To enable, uncomment the
# repo name header and Include lines. You can add preferred servers immediately
# after the header, and they will be used before the default mirrors.

#[core-testing]
#Include = /etc/pacman.d/mirrorlist

[core]
Include = /etc/pacman.d/mirrorlist

#[extra-testing]
#Include = /etc/pacman.d/mirrorlist

[extra]
Include = /etc/pacman.d/mirrorlist

# If you want to run 32 bit applications on your x86_64 system,
# enable the multilib repositories as required here.

#[multilib-testing]
#Include = /etc/pacman.d/mirrorlist

#[multilib]
#Include = /etc/pacman.d/mirrorlist

# An example of a custom package repository.  See the pacman manpage for
# tips on creating your own repositories.
#[custom]
#SigLevel = Optional TrustAll
#Server = file:///home/custompkgs
[archlinuxcn]
Server = https://mirrors.aliyun.com/archlinuxcn/$arch
Server = https://mirrors.tencent.com/archlinuxcn/$arch
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
Server = https://mirrors.bfsu.edu.cn/archlinuxcn/$arch
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
Server = https://mirrors.neusoft.edu.cn/archlinuxcn/$arch
# 
# pacman-key --lsign-key "farseerfc@archlinux.org"
# pacman -Sy archlinuxcn-keyring 
#[bioarchlinux]
### ISCAS
#Server = https://mirror.iscas.ac.cn/BioArchLinux/$arch
#Server = https://mirror.nju.edu.cn/bioarchlinux/$arch
### Nanjing University
#Server = http://mirror.nju.edu.cn/bioarchlinux/$arch
### Nanjing University of Posts and Telecommunications
#Server = http://mirrors.njupt.edu.cn/bioarchlinux/$arch
#Server = https://mirrors.njupt.edu.cn/bioarchlinux/$arch
### Shandong University
#Server = http://mirrors.sdu.edu.cn/bioarchlinux/$arch
#Server = https://mirrors.sdu.edu.cn/bioarchlinux/$arch
#[arch4edu]
#Server = https://mirrors.tuna.tsinghua.edu.cn/arch4edu/$arch
#[blackarch]
#Server = https://mirrors.tuna.tsinghua.edu.cn/blackarch/$repo/os/$arch
EOF
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