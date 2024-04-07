### 更新 wsl app verison

```bash
sudo apt update
sudo apt upgrade
```

### wget 下载安装包

**去**[官网](https://www.anaconda.com/download)复制下载链接

```bash
wget copyed-url
```

### 安装

```bash
bash Anaconda*-*-Linux-x86_64.sh
```

### 安装完成后

```bash
source ~/.bashrc
```

### 创建 py2 执行环境

```bash
conda create -n py27 python=2.7
conda activate py27
```

### macos brew 安装

1. 安装

```
brew install anaconda --cask
```

2. 添加环境变量

.zshrc
EXPORT=anaconda path

3. 初始化

```
conda init zsh
```
