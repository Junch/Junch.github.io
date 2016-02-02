---
layout: post
title: "使用mongodb shell script"
summary: ""
description: ""
tags: ["mongodb"]
categories: ["笔记"]
---

公司的一个内部产品使用了nodejs, mongodb。图表使用qplot。由于某个发布失误，图表中出现了几个特别大的值，结果整个图表就被压扁了。

QA老大有点‘洁癖’，希望修改这几个突兀的数值，让图形好看点。那就修改一下数据吧。

先执行命令mongo, 然后在shell中执行show dbs，use CERDataCache选择好数据库。接着执行下面的script

{% highlight javascript %}

// Show the data
for (var i=25; i<45: ++i) {
    var cur = db["e2873097_week" + i].find({table_name: "WeeklyReportCount"});
    printjson({i:i, val: cur.next().r1.f1});
}

// Update the data
db.e2873097_week34.update({table_name: "WeeklyReportCount"}, {
    $set: {
        "r1.f1": NumberInt(1300)
    }
}
});

{% endhighlight %}

有以下两点需要注意的地方

1. 修改整数值的时候，记得加上NumberInt，否则这个值会被当成字符串。

2. 如果document的名字以数字开始，那么下面的写法就是错误的，比如

{% highlight javascript %}
// Error
db.2873097_week34.update({table_name: "WeeklyReportCount"}, {
    $set: {
        "r1.f1": NumberInt(1300)
    }
}
});

// Correct
db["2873097_week34"].update({table_name: "WeeklyReportCount"}, {
    $set: {
        "r1.f1": NumberInt(1300)
    }
}
});

{% endhighlight %}