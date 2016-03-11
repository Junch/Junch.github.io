---
layout: post
title: "使用Nuget制作C++ Packages"
summary: "简单尝试"
description: ""
tags: ["nuget"]
categories: ["笔记"]
---
很多流行的语言都有包管理器，比如 Javascript 有 NPM，Ruby 有 Gem，Python 有 pip，C# 有NuGet。悲催的 C++ 没有任何通行的包管理器，这为开发带来了不便。

公司的高人突发奇想。既然 NuGet 的客户端可以将任何文件打包，那么 C++ 的头文件，库文件还有二进制文件也不在话下了。

## 下载 packages
尝试从官方网站 https://www.nuget.org 下载一个包

```bash
$ nuget.exe install cef.sdk -NoCache -NonInteractive -Source nuget.org
```
也可以尝试下面的命令，一次性安装多个 packages

```bash
$ nuget.exe restore project.json -NoCache -NonInteractive -Source nuget.org -OutputDirectory .
```

project.json的内容如下所示

```json
{
  'dependencies': {
    'asio-cpp' : '1.10.2', 
    'cef.sdk': '3.2454.1344'
  }, 

  'frameworks': {
    'native': {}
  }
}
```

## 制作 packages
假设 C++ 的代码在 C:\Plugin，下面有两个文件夹，分别是 include, lib。可以采用下面的方式来生成两个 packages。

**plugin-header.nuspec**

```xml
<?xml version="1.0"?>
<package >
  <metadata>
    <id>plugin-lib_win_$configuration$_$instructionset$_$toolchain$</id>
    <version>1.0.0</version>
    <authors>Test</authors>
    <owners>Test</owners>
    <description>plugin $platform$-bit $configuration$ libs</description>
    <dependencies>
      <dependency id="Plugin-header" version="$version$"/>
    </dependencies>
  </metadata>
  <files>
    <file src="lib-$platform$\**" target="lib"/>
  </files>
</package>
```

**plugin-lib.nuspec**

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/10/nuspec.xsd>
  <metadata>
    <id>plugin-lib_win_$configuration$_$instructionset$_$toolchain$</id>
    <version>1.0.0</version>
    <authors>Test</authors>
    <owners>Test</owners>
    <description>plugin $platform$-bit $configuration$ libs</description>
    <dependencies>
      <dependency id="Plugin-header" version="$version$"/>
    </dependencies>
  </metadata>
  <files>
    <file src="lib-$platform$\**" target="lib"/>
  </files>
</package>
```

执行如下命令来生成 lib 的 package *plugin-lib_win_debug_intel64_v140.1.0.0-A001M008.nupkg*

```batch
$ nuget pack plugin.nuspec -basepath c:\Plugin -version 1.0.0-A001M008 -properties configuration=debug;instructionset=intel64;toolchain=v140;platform=x64;bit=64
```

## Reference
- [Nuspec Reference](https://docs.nuget.org/create/nuspec-reference)
- [Nuget Command Line Reference](https://docs.nuget.org/consume/command-line-reference)
- [Semantic Versioning](http://semver.org/)
