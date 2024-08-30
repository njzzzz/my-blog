## windows下安装zsh

1. 安装gitbash

2. 下载[zsh](https://packages.msys2.org/package/zsh?repo=msys&variant=x86_64)

3. 解压

4. 将解压内容复制到gitbash的安装目录，一般为`C:\Program Files\Git`

5. 打开gitbash，进行zsh初始化

    ```bash
    zsh

    ```
6. 创建.bashrc文件，并写入

    ```bashrc
    if [ -t 1 ]; then
    exec zsh
    fi
    ```

7. 重启bash


8. 安装oh-my-zsh

    ``` bash
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

    ```