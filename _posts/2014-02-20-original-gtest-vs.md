---
layout: post
title: "在Visual Studio 2012上配置gmock"
tagline: ""
description: ""
tags: ["gtest", "gmock"]
categories: ["单元测试"]
---
周五要host一个关于TDD的会议，需要使用gtest。我平常都是在Mac或者Ubuntu上练习使用gtest，可考虑到同事们平常都使用VS，在VS上演示效果应该更好。经过一番努力配置成功，特将相关步骤总结如下。

gmock的安装包已经包含了gtest。安装gmock后gtest的所有功能都可以使用，另外还可以使用一些gmock的特性，比如Matcher。所以我直接安装了gmock。

### 1. 下载编译
从网上下载[gmock-1.7.0.zip][gmock]，解压后用VS打开msvc／2010目录下的工程文件gmock.sln。右键gmock打开属性对话框，在C/C++ > Preprocessor > Preprocessor Definitions属性中添加_VARIADIC_MAX=10。接着将General > Platform Toolset的选项设置成v110。编译连接成功。

gmock_main也同样配置。

### 2. 添加环境变量
系统环境变量中添加GMOCK_MAIN，指向gmock-1.7.0目录。

### 3. 配置Test工程
对于Test工程项目，需要在属性对话框中作如下配置：

1. C/C++ > Preprocessor > Preprocessor Definitions: 添加 _VARIADIC_MAX=10
2. C/C++ > General > Additional Include Directories: 添加 $(GMOCK_HOME)\include;$(GMOCK_HOME)\gtest\include;
3. C/C++ > Code Generation: Runtime library: /MTd
4. Linker > Input > Additional Dependency > 添加 gmock.lib 
5. Linker > General > Additional Library Directories: 添加 $(GMOCK_HOME)\msvc\2010\Debug;

上面的过程非常繁琐，下面提供一种简化的做法：

1. 下载[props.zip][props]文件并解压到$(GMOCK_HOME)目录。
2. C/C++ > Code Generation: Runtime library: /MTd
3. 用文本编辑器打开.vcxproj文件，将下面三行语句插入到 \</PropertyGroup>后面。

```xml
<ImportGroup Label="PropertySheets">
    <Import Project="$(GMOCK_HOME)\props\gmock.props" />
</ImportGroup>
```

注意：如果没有链接gmock_main.lib，你需要提供main函数如下所示。如果链接了gmock_main.lib，这个main函数也可以省掉了。

```cpp
#include <gmock/gmock.h>

int main(int argc,char** argv){
    testing::InitGoogleMock(&argc,argv);
    return RUN_ALL_TESTS();
}
```

### 4. 安装GoogleTest Runner
以上三个步骤可以让gtest/gmock跑起来了。GooglTest Runner是个VS插件，它让你感觉gtest/gmock仿佛是VS内置的单元测试框架。这里不再赘述，请访问[网站][GoogleTestRunner]。

[gmock]: https://googlemock.googlecode.com/files/gmock-1.7.0.zip
[props]: /download/props.zip
[GoogleTestRunner]: https://github.com/markusl/GoogleTestRunner
