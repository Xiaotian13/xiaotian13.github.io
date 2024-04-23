三个基础配置文件：

<br>

> launch.json

``` json
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: 当前文件",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "internalConsole",
            
            "python": "C:\\Users\\xiaotian13\\anaconda3\\python.exe",
            "cwd": "${workspaceRoot}",
            "env": {},
            "envFile": "${workspaceRoot}/.env",
            "stopOnEntry": false, //是否在第一条语句时程序停止，下面的这个选项都一样
            
        }
    ]
}
```

<br>

> settings.json

``` json
{
    "python.defaultInterpreterPath": "C:\\D\\file\\miniconda3\\envs\\quant-main\\python.exe",
    "editor.fontSize": 18,
    "editor.renderControlCharacters": true,
    "editor.renderWhitespace": "all",
    "editor.tabSize": 4,
    "editor.fontFamily": "'Microsoft YaHei', 'Courier New', monospace",
    "terminal.integrated.fontFamily": "monospace",
    "workbench.tree.enableStickyScroll": true,
    "notebook.lineNumbers": "on",
}
```

<br>

> tasks.json

``` json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "echo",
            "type": "shell",
            "command": "echo Hello"
        }
    ]
}
```

