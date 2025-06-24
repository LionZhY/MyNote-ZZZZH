

# vscode配置相关

> [vscode+ Cpp配置相关](https://blog.csdn.net/weixin_52159554/article/details/134406628#:~:text=VSCode%20%E5%AE%89%E8%A3%85%E5%A5%BD%E4%B9%8B%E5%90%8E%EF%BC%8C%E6%88%91%E4%BB%AC,%E5%BF%85%E9%A1%BB%E6%9C%89%E7%BC%96%E8%AF%91%E5%99%A8%E4%BD%BF%E7%94%A8%E3%80%82)

## **配置编译器**

![image-20250324205222463](pic/image-20250324205222463.png)



## 编译运行**cpp**

### **配置 `tasks.json` **【终端–配置任务】

- **按 `Ctrl+Shift+P`**，输入 `tasks: Configure Tasks` 并选择 `Create tasks.json file from template`，然后选择 `Others`。（或者直接**终端–配置任务**）

  ![image-20250324174012183](pic/image-20250324174012183.png)

  ![image-20250324205146638](pic/image-20250324205146638.png)

  ![image-20250324210347025](pic/image-20250324210347025.png)

### **编译**【终端–运行任务】

![image-20250324201944513](pic/image-20250324201944513.png)

![image-20250324212741893](pic/image-20250324212741893.png)



**注意修改tasks.json，添加`"-I${workspaceFolder}/include" `不然找不到头文件**

```c++
{
	"version": "2.0.0",
	"tasks": [
		{
			"type": "cppbuild",
			"label": "C/C++: g++-11 生成活动文件",
			"command": "/usr/bin/g++-11",
			"args": [
				"-fdiagnostics-color=always",
				"-g",
				"${file}",
				"-o",
				"${fileDirname}/${fileBasenameNoExtension}",
				"-I${workspaceFolder}/include" // 添加头文件路径!!!
			],
			"options": {
				"cwd": "${fileDirname}"
			},
			"problemMatcher": [
				"$gcc"
			],
			"group": "build",
			"detail": "编译器: /usr/bin/g++-11"
		}
	]
}
```



### 执行

打开vscode自带的终端【ctrl + `】，输入 【  ./ 可执行程序的名字 】

![image-20250324212117652](pic/image-20250324212117652.png)



### 重新编译 Ctrl + Shift + B

后面修改了代码之后，只需要重新编译【终端–运行任务】，不需要重写tasks.json



### 配置文件可以复用

如果在新文件夹里写新的代码，可以直接.vscode文件夹拷贝到新文件夹下，配置文件可以复用，直接【终端–运行任务】即可

![image-20250324212433973](pic/image-20250324212433973.png)

![image-20250324214054894](pic/image-20250324214054894.png)

![image-20250324214108777](pic/image-20250324214108777.png)





## 调试

### vscode

创建launch.json文件

![image-20250324215058659](pic/image-20250324215058659.png)

![image-20250324221230272](pic/image-20250324221230272.png)

```c++
//可执行文件应该和源码在 同一目录
"program": "${fileDirname}/${fileBasenameNoExtension}",

// 或者直接写明需要调试的文件（.exe）位置
"program": "/home/zhangyan/projects/muduo-learning/src/Timestamp",
```



回到.cpp文件，

F9 - 打断点/取消断点

F5 - 启动调试

F11 - 逐语句调试

F10 - 逐过程调试



### 手动调试

例如

```shell
gdb /home/zhangyan/projects/muduo-learning/src/Timestamp
```





## 头文件包含问题

![image-20250330173413625](pic/image-20250330173413625.png)

**更新 VS Code 的 `c_cpp_properties.json`**

1. 在 VS Code 里，打开 `.vscode/c_cpp_properties.json` 文件（如果没有，创建一个）。

2. 确保 `includePath` 包含 `include` 目录，例如：

   ~~~c++
   {
       "configurations": [
           {
               "name": "Linux",
               "includePath": [ // 头文件搜索路径
                   // "${workspaceFolder}/**"
                   "${workspaceFolder}/include", // 项目include目录
                   "${workspaceFolder}/src",     // 项目src目录
                   "/usr/include",               // 系统标准头文件路径
                   "/usr/local/include"          // 额外安装的库的头文件路径
               ],
               "defines": [], // 预处理定义宏
               "compilerPath": "/usr/bin/g++-11", // 编译器路径
               "cStandard": "c17",
               "cppStandard": "gnu++17",
               "intelliSenseMode": "linux-gcc-x64"
           }
       ],
       "version": 4
   }
   ~~~

   

