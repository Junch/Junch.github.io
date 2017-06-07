---
layout: post
title: "Debug on the iOS Simulator with LLDB"
summary: "使用lldb在iOS Simulator上调试app"
description: ""
tags: ["iOS", ""]
categories: ["Debug"]
---
I've built a iOS App named *Calculator.app*. Its bundle identifier is *test.game.org.Calculator*. I want to debug it with LLDB on iOS Simulator iPhone 6. The steps are as below.

#### 1. Get the device UUID

```bash
xcrun simctl list devices #Query the devices
```

You may get the result as below

```
== Devices ==
-- iOS 9.3 --
    iPhone 4s (26938B51-CC13-438D-B2BC-4F9E0A38E370) (Shutdown)
    iPhone 5 (61020A8C-5374-4352-B3B3-7D837A5920D1) (Shutdown)
    iPhone 5s (018882CB-6AC2-4785-AF30-4DF43A82DA4D) (Shutdown)
    iPhone 6 (72910B8C-9149-4260-8552-023E8543A0C9) (Shutdown)
    iPhone 6 Plus (46841071-113D-4CCD-9CA3-2E10BE36B818) (Shutdown)
    iPhone 6s (7DAC5FCF-0D2D-4BD5-ACCA-3E79884F5FE9) (Shutdown)
    iPhone 6s Plus (9C0092B2-AF0B-447F-AB0E-33B69836F659) (Shutdown)
    ...
```

The UUID for the iPhone 6 is 72910B8C-9149-4260-8552-023E8543A0C9.

#### 2. Launch the Simulator 

```bash
open -a Simulator --args -CurrentDeviceUDID 72910B8C-9149-4260-8552-023E8543A0C9 #iPhone 6
```

#### 3. Install the App

```bash
xcrun simctl install booted Calculator.app #path to your.app
```

#### 4. Launch LLDB and wait for the App to launch

```bash
lldb
(lldb) platform select ios-simulator
    . . .
(lldb) platform connect 72910B8C-9149-4260-8552-023E8543A0C9
(lldb) process attach -n Calculator --waitfor
```

You should not use the bundle identifier *test.game.org.Calculator* here on the **process attach** command. You can execute lldb command **help process attach** for more information. 

#### 5. Launch the App

```bash
xcrun simctl launch booted test.game.org.Calculator
```

The App name should use the bundle identifier. The App will be launched and at the same time LLDB will attach to the process Calculator.

If your App need arguments, you can add the arguments immediately

```bash
xcrun simctl launch booted test.game.org.Calculator --gtest_filter=LoginCloudSecureMemoryTests.LoginWithWrongUsername
```

#### 6. Use script
The lldb commands can be documented in a file, for example *debug.cmd*. 

```bash
# lldb.cmd
platform select ios-simulator
platform connect 72910B8C-9149-4260-8552-023E8543A0C9
process attach -n Calculator --waitfor
br s -F main
process continue
```
Execute the script as below

```bash
lldb --source debug.cmd
```