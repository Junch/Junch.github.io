---
layout: post
title: "学习Asio (1)"
tagline: "Mac下设置开发环境"
description: ""
tags: ["asio", "boost", "cmake"]
categories: [""]
---
{% include JB/setup %}

###编译boost

在Mac下使用brew编译boost，为了使用C++11，需要加入参数--c++11

{% highlight bash %}
$ brew install boost --c++11
{% endhighlight %}

在我的Mac虚拟机里面用了20分钟左右编译成功。

###示例代码
服务器代码：

{% highlight cpp %}
#include <iostream>
#include <boost/asio.hpp>
 
using namespace boost::asio;
 
int main(int argc, char* argv[])
{
    // 所有asio类都需要io_service对象
    io_service iosev;

    ip::tcp::acceptor acceptor(iosev, 
        ip::tcp::endpoint(ip::tcp::v4(), 10000));

    for(;;)
    {
         // socket对象
        ip::tcp::socket socket(iosev);
        // 等待直到客户端连接进来
        acceptor.accept(socket);
 
        // 显示连接进来的客户端
        std::cout << socket.remote_endpoint().address() << std::endl;
        // 向客户端发送hello world!
        boost::system::error_code ec;
        socket.write_some(buffer("hello world!\n"), ec);
 
        // 如果出错，打印出错信息
        if(ec)
        {
            std::cout << 
                boost::system::system_error(ec).what() << std::endl;
            break;
        }
        // 与当前客户交互完成后循环继续等待下一客户连接
    }
    return 0;
}
{% endhighlight %}

客户端代码：

{% highlight cpp %}
#include <iostream>
#include <boost/asio.hpp>
 
using namespace boost::asio;
 
int main(int argc, char* argv[])
{
    // 所有asio类都需要io_service对象
    io_service iosev;
    // socket对象
    ip::tcp::socket socket(iosev);
    // 连接端点，这里使用了本机连接，可以修改IP地址测试远程连接
    ip::tcp::endpoint ep(ip::address_v4::from_string("127.0.0.1"), 10000);
    // 连接服务器
    boost::system::error_code ec;
    socket.connect(ep,ec);
    // 如果出错，打印出错信息
    if(ec)
    {
        std::cout << boost::system::system_error(ec).what() << std::endl;
        return -1;
    }
    // 接收数据
    char buf[100];
    size_t len=socket.read_some(buffer(buf), ec);
    std::cout.write(buf, len);
 
    return 0;
}
{% endhighlight %}

###编写CMake脚本

{% highlight cmake %}
CMAKE_MINIMUM_REQUIRED (VERSION 2.6)

PROJECT (HELLO)

SET(CMAKE_CXX_FLAGS "-std=c++11 -stdlib=libc++ -Wall -O2")

FIND_PACKAGE(Boost 1.55.0 REQUIRED COMPONENTS system thread regex)
IF(Boost_FOUND)
  INCLUDE_DIRECTORIES(${Boost_INCLUDE_DIRS})
  LINK_DIRECTORIES(${Boost_LIBRARY_DIRS})
ENDIF(Boost_FOUND)

SET(USED_LIBS ${Boost_SYSTEM_LIBRARY} ${Boost_THREAD_LIBRARY} ${Boost_REGEX_LIBRARY})

ADD_EXECUTABLE(server server.cpp)
TARGET_LINK_LIBRARIES(server ${USED_LIBS})

ADD_EXECUTABLE(client client.cpp)
TARGET_LINK_LIBRARIES(client ${USED_LIBS})
{% endhighlight %}

###关闭server进程

{% highlight bash %}
$ ps -A | grep [name of program you want to close]
$ kill -9 [process id]
{% endhighlight %}

###下载代码
[代码](https://github.com/Junch/asio/tree/master/hello)
