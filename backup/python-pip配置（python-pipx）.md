pipx只能使用二进制作为全局工具，不适合作为库来调用。
因此，还是要安装pip。

## 安装
```shell
sudo pacman -S python-pip python-pipx #安装pip和pipx
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple #配置国内镜像
pip install name --user
```

## 创建并激活虚拟环境
```shell
python -m venv envname
source envname/bin/activate
```

## 参考

- [Python/Virtual environment - ArchWiki](https://wiki.archlinux.org/title/Python/Virtual_environment)