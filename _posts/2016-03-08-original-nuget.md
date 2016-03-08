---
layout: post
title: "使用Nuget制作C++ Packages"
summary: "简单尝试"
description: ""
tags: ["nuget"]
categories: ["笔记"]
---
很多流行的语言都有包管理器，比如Javascript有NPM，Ruby有Gem，Python有pip，C#有NuGet。悲催的C++没有任何通行的包管理器，这为开发带来了不便。

公司的高人突发奇想。既然NuGet的客户端可以将任何文件打包，那么C++的头文件，库文件还有二进制文件也不在话下了。

假设C++的代码在C:\Plugin，下面有两个文件夹，分别是include, lib。可以采用下面的方式来生成两个package。

## 头文件的package spec
plugin-header.nuspec

{% highlight xml %}
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/10/nuspec.xsd>
  <metadata>
    <id>plugin-header</id>
    <version>1.0.0</version>
    <authors>Test</authors>
    <owners>Test</owners>
    <description>Plugin App</description>
  </metadata>
  <files>
    <file src="include\**" target="include"/>
  </files>
</package>
{% endhighlight %}

## 库文件的package spec
plugin-lib.nuspec

{% highlight xml %}
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
{% endhighlight %}

执行如下命令来生成lib的package *plugin-lib_win_debug_intel64_v140.1.0.0-A001M008.nupkg*

{% highlight batch %}
nuget pack plugin.nuspec -basepath c:\Plugin -version 1.0.0-A001M008 -properties configuration=debug;instructionset=intel64;toolchain=v140;platform=x64;bit=64
{% endhighlight %}

## Reference
- [Nuspec Reference](https://docs.nuget.org/create/nuspec-reference)
- [Nuget Command Line Reference](https://docs.nuget.org/consume/command-line-reference)
- [Semantic Versioning](http://semver.org/)
