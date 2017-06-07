---
layout: post
title: "使用os_log"
summary: "Efficiently capture log messages to memory and disk. Manage logging behavior and persistence."
description: ""
tags: ["iOS", ""]
categories: ["Debug"]
---

调试iOS Simulator中运行的代码比较繁琐，参见[使用lldb在iOS Simulator上调试app]({{ site.baseurl }}{% post_url 2016-09-19-original-lldb %})。另一种办法是打log，相对好操作。

```cpp
// testlog.cpp
#include <os/log.h>

int main(void)
{
    os_log_fault(OS_LOG_DEFAULT, "Hello os_log_fault");
    os_log_error(OS_LOG_DEFAULT, "Hello os_log_error");
    os_log_info(OS_LOG_DEFAULT, "Hello os_log_info");   // Not shown on console
    os_log_debug(OS_LOG_DEFAULT, "Hello os_log_debug"); // Not shown on console

    os_log(OS_LOG_DEFAULT, "Hello os_log");
    os_log(OS_LOG_DEFAULT, "Hello os_log %d", 5);
    os_log_with_type(OS_LOG_DEFAULT, OS_LOG_TYPE_DEFAULT, "Hello os_log_with_type");
    const char* msg = "Hello os_log_with_type dynamic";
    os_log_with_type(OS_LOG_DEFAULT, OS_LOG_TYPE_DEFAULT, "%s %d", msg, 5);

    return 0;
}
```

将上面的例子编译后执行

```bash
clang++ testlog.cpp -o testlog
./testlog
```

在console中可以查看到log

![](/images/oslog.png)

更多内容可以参考

- Apple文档：[Logging](https://developer.apple.com/documentation/os/logging?language=objc)
- 视频：[WWDC 2016: Unified Logging and Activity Tracing](https://developer.apple.com/videos/play/wwdc2016/721/?time=2583)
- [iOS Unified Logging](https://gist.github.com/otto-schnurr/696483097c2d84e8518886203bdad154)