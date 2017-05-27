---
layout: post
title: "Oracle 杂记"
summary: "Oracle 学习杂记"
description: ""
tags: ["oracle", "database"]
categories: ["笔记"]
---

## 访问权限

我对CER数据库的访问权限是只读，而且发现在SQL Developer中甚至不能展开Tables。

![](/images/cer_oracle.png)

执行下面的query，我得到查询结果： All Rows Fetched: 0 in 0.109 seconds。
```sql
SELECT * FROM USER_OBJECTS;
SELECT * FROM USER_SYNONYMS;
```

后来使用下面的query才得到自己想要的结果。

```sql
SELECT * FROM ALL_SYNONYMS;
```

查询文档有下列解释：

1. **ALL_SYNONYMS** describes the synonyms accessible to the current user.
2. **USER_SYNONYMS** describes the synonyms owned by the current user.
3. **USER_OBJECTS** describes all objects owned by the current user.

----

## 注释

- 单行注释：---
- 多行注释：/* */
```sql
--SELECT * FROM USER_OBJECTS;
SELECT * FROM USER_SYNONYMS;
/*
SELECT * FROM tb_name;
DBMS_OUTPUT.PUT_LINE('Hello World');
*/
```