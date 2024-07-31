# vscode `chrome/node` attach 调试方法，保留用户数据和插件，但窗口，多项目调试

## Node Attcah
1. 配置`.vscode/launch.json`
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      // 此处配置为attach
      "request": "attach",
      "name": "debugger:services",
      "skipFiles": ["<node_internals>/**"],
      "cwd": "${workspaceFolder}",
      // 使用sourceMap
      "sourceMaps": true,
      // 设置PickPorcess
      "processId": "${command:PickProcess}",
      // 设置调试端口,这个端口可以自己设置
      "port": 9230,
      // 设置进程重启时重新attach
      "restart": true
    }
  ]
}
```
2. 修改package.josn 启动命令inpect端口为上述`9230`, 此处以nodemon为例
```json
"scripts": {
    "debug:dev": "nodemon --inspect=9230 server/app.js",
},
```

3. 执行`npm run debug:dev`

4. 点击vscode debug 选项，选中debugger:services，再选中第二步启动的进程

## Chrome Attach  【配置完成后关闭所有chrome进程，windows可通过任务管理器关闭，然后点击快捷方式重新打开chrome】

1. 修改chrome启动参数，增加`--remote-debugging-port=9229` （指定调试端口）`--user-data-dir=your user data`（指定使用的用户数据，以保证插件等环境完整）
    - win 
        右键chrome快捷方式，在目标后增加参数  
        "C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9229 --user-data-dir="C:\Users\xxxx\AppData\Local\Google\Chrome\User Data"
    - mac
        mac可使用命令行打开，或自行增加alias  
        alias chrome='/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome  --remote-debugging-port=9229 --user-data-dir="xxxxx“'

2. 配置`.vscode/launch.json`
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "attach",
      "name": "debugger:react",
      "url": "http://localhost:3000",
      "port": 9229,
      "webRoot": "${workspaceFolder}",
      "sourceMaps": true,
      "restart": true
    }
  ]
}

```
3. 启动项目如`npm run dev`

4. 点击vscode debug 选项，选中debugger:react
