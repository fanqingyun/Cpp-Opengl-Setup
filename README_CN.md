假设使用Windows平台，VSCode并通过MSYS2安装MinGW。其他平台上的VSCode用户也可以阅读本文。

# 1. C++

可以看这篇文章:

[C/C++ for Visual Studio Code](https://code.visualstudio.com/docs/languages/cpp)

设置完成后，你可以在VSCode中打开此项目中的`hello_world`文件夹，运行`hello_world.cpp`。如果你在屏幕中看到"Hello, world!"，则表示你已成功设置C++环境。

# 2. OpenGL

## 2.1. 下载

在Windows中，如果你能在C:/Windows/System32中找到`opengl32.dll`，那么你已经安装了OpenGL。其他平台也类似。如果没有，请阅读以下内容：

[Downloading OpenGL](https://www.khronos.org/opengl/wiki/Getting_Started#Downloading_OpenGL)

除了 OpenGL，还需要另外两个库来帮助你使用 OpenGL，一个是窗口工具包，另一个是 OpenGL 加载库。在本文中，我们选择GLFW和GLAD。

## 2.2. GLFW

去官方网站[GLFW](https://www.glfw.org/download.html)下载。如果你是经验丰富的开发人员，则可以下载源码包并使用 CMake 从源码构建它。

如果你是初学者，则可以下载预编译的二进制文件，这些二进制文件有两个版本适用于Windows和MacOS。如果你使用的是Linux，你可以从你的软件包系统中获取GLFW，例如，在Ubuntu上的apt。

下载 zip 文件后，将其解压缩。

现在，你可以使用此项目文件夹`Cpp-OpenGL-Setup-main`(下文简称OpenGL/)，并在其中创建两个子文件夹。一个是“include”，用于.h头文件，另一个是“lib”，用于库文件，例如.dll/.a/.c等。

将 glfw-3.x.x.bin.WIN64(或 MACOS)/include/ 中的内容移动到 OpenGL/include/，将 glfw-3.3.8.bin.WIN64/lib-mingw-w64/ 中的内容移动到 OpenGL/lib/GLFW/(需创建)，“lib-mingw-w64”因你在 Windows 上的编译器(例如 lib-vc2019)或 MacOS 上的硬件架构(例如 lib-x86_64)而异。

## 2.3. GLAD

阅读此处的最后一节“Setting up GLAD”：

[Learn OpenGL](https://learnopengl.com/Getting-started/Creating-a-window)

将 glad/include/ 移动到 OpenGL/include/， glad/src/glad.c 到 OpenGL/lib/

## 2.4. VSCode

现在你的计算机上有了 GLFW 和 GLAD ，我们需要配置 VSCode。

此项目中的`.vscode`文件夹中有四个文件：c_cpp_properties.json，launch.json，tasks.json和settings.json。如果在安装 MSYS2 时选择默认文件夹 (C:/msys64)，则可以直接使用它们。但我需要在这里做一些说明。

你需要配置 2 个文件夹：include和lib。

1\) include

所有文件均为 .h 文件。你应该在 c_cpp_properties.json 文件的“includePath”中添加：

"${workspaceFolder}/include/**"

"\*\*"表示在文件夹中以递归方式搜索。

然后你应该在tasks.json的"args"中添加：

"-I",

"${workspaceFolder}\\\\include"

来使其生效。

2\) lib

首先你应该在 c_cpp_properties.json 文件的“includePath”中添加：

"${workspaceFolder}/lib/**"

以下有关 lib 的配置都在 tasks.json 的“args”中。

对于 .a/.c 文件，将它们放在“-g”之后，例如：

"-g",

"${workspaceFolder}\\\\lib\\\\GLFW\\\\**.a",

"${workspaceFolder}\\\\lib\\\\**.c",

\*\*.a 表示文件夹中扩展名为 .a 的所有文件。请注意，如果你的输入后还有其他行，请添加“,”。

对于.dll文件，你首先需要告诉 VSCode 在哪里可以找到它们，请使用“-L”：

"-L",

"${workspaceFolder}\\\\lib\\\\GLFW",

然后使用 “-l” 链接到.dll文件，注意对于每个.dll文件之前都需要一个 “-l”，你可以省略.dll扩展名：

"-l",

"glfw3",

现在你已经配置好了VSCode中的GLFW 和 GLAD ，但你需要一些其他.dll文件才能让你的程序正常运行。

它们是 C:/Windows/System32 中的.dll文件，因此你无需使用“-L”指定文件夹，只需添加它们即可：

"-l",

"opengl32", 

"-l",

"gdi32",

同样，还有“kernel32.dll”、“user32.dll”等。如果需要，请添加它们。

注意这些都是.dll文件，.lib和.dylib如何配置需要你自己尝试。

顺便说一下，如果你在其他目录中安装 MYSY2，你应该手动修改这些 json 文件中 g++.exe/gcc.exe/gdb.exe 的路径。

# 3. 结语

现在你已经完成了一切。目录树如下：

```
Cpp-OpenGL-Setup-main
│
│  .gitignore
│  hello_hexagonal_star.cpp
│  LICENSE
│  NOTICE
│  README.md
│  README_CN.md
│  
├─.vscode
│      c_cpp_properties.json
│      launch.json
│      settings.json
│      tasks.json
│      
├─hello_world
│  │  hello_world.cpp
│  │  
│  └─.vscode
│          c_cpp_properties.json
│          launch.json
│          settings.json
│          tasks.json
│          
├─include
│  ├─glad
│  │      glad.h
│  │      
│  ├─GLFW
│  │      glfw3.h
│  │      glfw3native.h
│  │      
│  └─KHR
│          khrplatform.h
│          
└─lib
    │  glad.c
    │  
    └─GLFW
            glfw3.dll
            libglfw3.a
            libglfw3dll.a
```

然后在VSCode中打开`Cpp-OpenGL-Setup-main`文件夹，运行`hello_hexagonal_star.cpp`. 如果你得到一个六茫星，那么你已成功搭建OpenGL环境：

![](https://pic2.zhimg.com/80/v2-154375a9d0d2e84b3e4c4867c58f8351_720w.webp)

# 4. 协议

此项目使用 [BSD-3-Clause](/LICENSE) 协议，第三方库的协议在此文件中：[NOTICE](/NOTICE)。
