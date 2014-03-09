---
layout: post
title: "在Visual Studio 2012上配置gmock续"
tagline: ""
description: ""
tags: ["gtest", "gmock"]
categories: ["单元测试"]
---
{% include JB/setup %}

我对使用Visual Studio project properties做项目配置一直很感兴趣，但这方面的资料不多。昨天偶尔遇到了一篇好文章，觉得有必要记录一下。这篇[文章][stackoverflow]在stackoverflow上, 关于配置一个工程的输出目录和中间文件目录。这个例子实在太好，我就抄录如下：

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup Label="UserMacros">
    <!--IsDebug: search for 'Debug' in Configuration-->
    <IsDebug>$([System.Convert]::ToString( $([System.Text.RegularExpressions.Regex]::IsMatch($(Configuration), '[Dd]ebug'))))</IsDebug>

    <!--ShortPlatform-->
    <ShortPlatform Condition="'$(Platform)' == 'Win32'">x86</ShortPlatform>
    <ShortPlatform Condition="'$(Platform)' == 'x64'">x64</ShortPlatform>

    <!--build parameters-->
    <BUILD_DIR>$(registry:HKEY_CURRENT_USER\Software\MyCompany\@BUILD_DIR)</BUILD_DIR>
  </PropertyGroup>

  <Choose>
    <When Condition="$([System.Convert]::ToBoolean($(IsDebug)))">
      <!-- debug macroses -->
      <PropertyGroup Label="UserMacros">
        <MyOutDirBase>Debug</MyOutDirBase>
        <DebugSuffix>-d</DebugSuffix>
      </PropertyGroup>
    </When>
    <Otherwise>
      <!-- other/release macroses -->
      <PropertyGroup Label="UserMacros">
        <MyOutDirBase>Release</MyOutDirBase>
        <DebugSuffix></DebugSuffix>
      </PropertyGroup>
    </Otherwise>
  </Choose>

  <Choose>
    <When Condition="Exists($(BUILD_DIR))">
      <PropertyGroup Label="UserMacros">
        <MyOutDir>$(BUILD_DIR)\Bin\$(MyOutDirBase)_$(ShortPlatform)\</MyOutDir>
        <MyIntDir>$(BUILD_DIR)\Build\$(Configuration)_$(ShortPlatform)_$(PlatformToolset)\$(ProjectGuid)\</MyIntDir>
      </PropertyGroup>
    </When>
    <Otherwise>
      <PropertyGroup Label="UserMacros">
        <MyOutDir>$(SolutionDir)\Bin\$(MyOutDirBase)_$(ShortPlatform)\</MyOutDir>
        <MyIntDir>$(SolutionDir)\Build\$(Configuration)_$(ShortPlatform)_$(PlatformToolset)\$(ProjectGuid)\</MyIntDir>
      </PropertyGroup>
    </Otherwise>
  </Choose>

  <PropertyGroup>
    <OutDir>$(MyOutDir)</OutDir>
    <IntDir>$(MyIntDir)</IntDir>
<!-- some common for projects
    <CharacterSet>Unicode</CharacterSet>
    <LinkIncremental>false</LinkIncremental>
--> 
  </PropertyGroup>
</Project>
{% endhighlight %}

###更新gmock.props文件

上篇文章所附的配置文件非常简单，如下所示：

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <ItemDefinitionGroup>
    <ClCompile>
      <PreprocessorDefinitions>_VARIADIC_MAX=10;%(PreprocessorDefinitions)</PreprocessorDefinitions>
	  <!-- Use $() instead of %% here as visual studio's editor doesn't recognize %% -->
      <AdditionalIncludeDirectories>$(GMOCK_HOME)\include;$(GMOCK_HOME)\gtest\include;%(AdditionalIncludeDirectories)</AdditionalIncludeDirectories>
    </ClCompile>
    <Link>
      <AdditionalDependencies>$(GMOCK_HOME)\msvc\2010\Debug\gmock.lib;%(AdditionalDependencies)</AdditionalDependencies>
    </Link>
  </ItemDefinitionGroup>
</Project>
{% endhighlight %}

上面的配置文件仅仅配置了Debug模式，显然在Release模式下无法正常工作。另外链接库也是可以指定的。参考Stackoverflow上的例子，我将配置文件更新如下：

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup Label="UserMacros">
    <!--IsDebug: search for 'Debug' in Configuration-->
    <IsDebug>$([System.Convert]::ToString( $([System.Text.RegularExpressions.Regex]::IsMatch($(Configuration), '[Dd]ebug'))))</IsDebug>
  </PropertyGroup>

  <Choose>
    <When Condition="$([System.Convert]::ToBoolean($(IsDebug)))">
      <!-- debug macroses -->
      <PropertyGroup Label="UserMacros">
        <GMockConfig>Debug</GMockConfig>
        <GMockRuntime>MultiThreadedDebug</GMockRuntime>
      </PropertyGroup>
    </When>
    <Otherwise>
      <!-- other/release macroses -->
      <PropertyGroup Label="UserMacros">
        <GMockConfig>Release</GMockConfig>
        <GMockRuntime>MultiThreaded</GMockRuntime>
      </PropertyGroup>
    </Otherwise>
  </Choose>

  <ItemDefinitionGroup>
    <ClCompile>
      <PreprocessorDefinitions>_VARIADIC_MAX=10;%(PreprocessorDefinitions)</PreprocessorDefinitions>
	    <!-- Use $() instead of %% here as visual studio's editor doesn't recognize %% -->
      <AdditionalIncludeDirectories>$(GMOCK_HOME)\include;$(GMOCK_HOME)\gtest\include;%(AdditionalIncludeDirectories)</AdditionalIncludeDirectories>
      <RuntimeLibrary>$(GMockRuntime)</RuntimeLibrary>
    </ClCompile>
    <Link>
      <AdditionalDependencies>$(GMOCK_HOME)\msvc\2010\$(GMockConfig)\gmock.lib;%(AdditionalDependencies)</AdditionalDependencies>
    </Link>
  </ItemDefinitionGroup> 
</Project>
{% endhighlight %}

###下载[配置文件][props]

[props]: {{ ASSET_PATH }}../download/props2.zip
[stackoverflow]:http://stackoverflow.com/questions/3502530/using-visual-studio-project-properties-effectively-for-multiple-projects-and-con



