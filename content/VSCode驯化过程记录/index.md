+++
title = "VSCode驯化过程记录"
date = 2022-01-23 22:57:09
slug = "202201232257"

[taxonomies]
tags = ["IDE" ]
categories = ["IDE"]

+++

换电脑，获得一只野生的VSCode

<!-- more -->

## 自动保存

**【Ctrl】+【，】**，打开常用设置，第一行就是`File:Auto Save`

想知道有多少受害者（X

懒人选择`afterDelay`，上方搜索框直接搜`auto save delay`设置延迟时间



## MinGW

确保环境变量配置正确：

```bash
g++ --version
gdb --version
```

打开VS Code，安装插件C/C++

然后**【Ctrl】+【Shift】+【P】**，打开命令面板，输入关键词"C/C++"

选中**编辑配置 (UI)**

设置**编译器路径**和**IntelliSense模式**

编写C程序选择`gcc.exe`，C++选择`g++.exe`

IntelliSense模式选择`gcc-x64`。（windows默认为windows-msvc-x64）

### 编译运行

第一次执行编译任务前，需要先进行配置

选择菜单栏【终端】→【配置任务…】

在弹出的待选项中按需求选择gcc或g++

之后会自动创建一个`tasks.json`文件，放在`.vscode`中

同目录下还有一个`launch.json`文件需要配置

#### tasks.json

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++.exe build active file",
            "command": "C:/msys64/mingw64/bin/g++.exe",
            "args": [
                "-g",
                "${file}",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe"
            ],
            "options": {
                "cwd": "${fileDirname}"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": "build",
            "detail": "compiler: C:/msys64/mingw64/bin/g++.exe"
        }
}
```

- command和detail可以不用绝对路径

#### launch.json

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "g++.exe - Build and debug active file",
      "type": "cppdbg",
      "request": "launch",
      "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
      "args": [],
      "stopAtEntry": false,
      "cwd": "${fileDirname}",
      "environment": [],
      "externalConsole": false,
      "MIMode": "gdb",
      "miDebuggerPath": "C:\\msys64\\mingw64\\bin\\gdb.exe",
      "setupCommands": [
        {
          "description": "Enable pretty-printing for gdb",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        }
      ],
      "preLaunchTask": "C/C++: g++.exe build active file"
    }
  ]
}
```

- miDebuggerPath改成本地绝对路径
- preLaunchTask和tasks.json的label对应

打开要编译的代码文件，选择菜单栏【终端】→【运行生成任务...】

或者 **【Ctrl】+【Shift】+【B】**

在弹出的待选项中选择之前生成的任务

运气好的话目录下已经有.exe文件了

好麻烦，还不如直接用命令行（X

但是有个坑：需要用`.\`指明它是当前目录下的可执行文件，因为VS Code的默认终端实际上是PowerShell。
