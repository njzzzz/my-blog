## Powershell7
### 获取可修改配置和快捷键
```ps1
Get-PSReadLineOption
Get-PSReadlineKeyHandler
```
全局生效
```ps1
// 获取配置文件
$PROFILE
// 加入这两行
Set-PSReadlineKeyHandler -Chord Ctrl+LeftArrrow BackwardWord
Set-PSReadlineKeyHandler -Chord Ctrl+RightArrrow ForwardWord
```
### 切换语言
```ps1
Update-Help -UICulture zh-CN
Update-Help -UICulture en-US
```