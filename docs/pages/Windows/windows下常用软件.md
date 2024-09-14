## windows下常用软件

1. [clash verge](https://clash-verge-rev.github.io/)
2. [everything](https://www.voidtools.com/zh-cn/downloads/)
3. [图吧工具箱](https://www.tbtool.cn/)
4. [进程监控工具procexp](https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer)
5. [端口管理工具tcpview](https://learn.microsoft.com/en-us/sysinternals/downloads/tcpview)
6. [host切换工具switchhost](https://switchhosts.vercel.app/zh)
7. [windows版redis memurai](https://www.memurai.com/get-memurai)
    
    win + r services.msc 找到memurai服务手动关闭
8. [WindowsTernimal](https://github.com/microsoft/terminal)
9. [powershell7](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.4&viewFallbackFrom=powershell-7&WT.mc_id=THOMASMAURER-blog-thmaure)

    设置ctrl + arrow 移动单词设置 
    ```
    Set-PSReadLineKeyHandler -Chord Ctrl+LeftArrow BackwardWord
    Set-PSReadLineKeyHandler -Chord Ctrl+RightArrow ForwardWord
    ```
10. java android 环境使用idea android studio
11. [开发环境下的https安全证书](https://github.com/FiloSottile/mkcert)
```bash
mkcert example.com "*.example.com" example.test localhost 127.0.0.1 ::1

mkcert --install
```
安装完成后需要手动信任证书,目录为
```bash
mkcert --CAROOT
```
![alt text](image.png)
![alt text](image-1.png)

12. [剪切板工具ditto](https://ditto-cp.sourceforge.io/)
13. [学习用激活工具win](https://github.com/zbezj/HEU_KMS_Activator/releases/download/42.0.4/HEU_KMS_Activator_v42.0.4.rar)
14. [学习用激活工具jetb](https://github.com/njzzzz/Jetbrains-Help)
15. [pws美化工具](https://starship.rs/zh-CN/guide/)
16. [win版本互转](https://github.com/TGSAN/CMWTAT_Digital_Edition/releases)
17. [截图工具](https://www.snipaste.com/)
18. [windows devtools 集合](https://learn.microsoft.com/en-us/windows/dev-environment/)


## 浏览器插件

### Locatorjs

#### 配置

1. vsocde

    需要自己在file后加上`/`

    ```bash
    vscode://file/${projectPath}${filePath}:${line}:${column}
    ```
2. webstorm

    webstorm使用如下命令可以打开文件

    ```bash
    webstorm64.exe --line linenumber --column columnnumber ${filepath}
    ```

> 修复webstorm使用toolbox安装可能没有注册urlschema导致无法打开

1. 新建文件夹 `C:/Registry`

2. 创建文件 `webstorm_open_schema.ps1` 用于解析路径

    ```ps1
    param (
        [string]$url
    )
    # Initialize variables
    $file = ""
    $line = ""
    $column = ""

    # Extract file parameter
    if ($url -match "file=([^&]+)") {
        $file = $matches[1]
    }

    # Extract line parameter
    if ($url -match "line=([^&]+)") {
        $line = $matches[1]
    }

    # Extract column parameter
    if ($url -match "column=([^&]+)") {
        $column = $matches[1]
    }

    # Remove URL encoding from the file path if necessary
    $file = [System.Uri]::UnescapeDataString($file)

    # Display extracted values for debugging (optional)
    Write-Host "File: $file"
    Write-Host "Line: $line"
    Write-Host "Column: $column"

    # Ensure WebStorm is installed at this path
    # ！！！！自行修改此处的路径
    $webstormPath = "C:\Users\gz_nj\AppData\Local\Programs\WebStorm\bin\webstorm64.exe"

    # Call WebStorm with the extracted parameters
    Start-Process "$webstormPath" "--line $line --column $column `"$file`""

    ```

3. 创建文件 `run_webstorm_open.vbs` 用于在后台执行ps1脚本

    ```vbs
    Set objShell = CreateObject("WScript.Shell")
    objShell.Run "powershell -ExecutionPolicy Bypass -File ""C:\Registry\webstorm_open_schema.ps1"" """ & WScript.Arguments(0) & """", 0, False
    ```

4. 创建注册表文件 `webstorm_open.reg` 用于支持 url schema

    ```reg
    Windows Registry Editor Version 5.00

    [HKEY_CLASSES_ROOT\webstorm]
    @="URL:WebStorm Protocol"
    "URL Protocol"=""

    [HKEY_CLASSES_ROOT\webstorm\DefaultIcon]
    ！！！！自行修改此处的路径
    @="C:\\Users\\gz_nj\\AppData\\Local\\Programs\\WebStorm\\bin\\webstorm64.exe,1"

    [HKEY_CLASSES_ROOT\webstorm\shell]

    [HKEY_CLASSES_ROOT\webstorm\shell\open]

    [HKEY_CLASSES_ROOT\webstorm\shell\open\command]
    @="wscript \"C:\\Registry\\run_webstorm_open.vbs\" \"%1\""

    ```