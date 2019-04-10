---
layout: blog
title: 'MySQL外键约束OnDelete和OnUpdate的使用'
excerpt: 'MySQL基础知识'
category: mysql
istop: true
isLast: true
code: true
date: 2019-01-12
background-image: https://images.unsplash.com/photo-1529101091764-c3526daf38fe?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=500&h=300&q=80
background-external: true
---

`On Delete` 和 `On Update` 都有 `Restrict`, `No Action`, `CasCade`, `Set Null` 属性，现在分别对他们的属性含意做个解释。

### ON DELETE

-   <b>restrict(约束)：</b>当在父表（即外键的来源表）中删除对应的记录时，首先检查是否有对应的外键，如果有则不允许删除

-   <b>no action：</b>意思同 restrict 一样，即如果存在从数据，不允许删除主数据

-   <b>cascade(级联)：</b>当在父表（即外键的来源表）中删除对应的记录时，首先检查是否有对应的外键，如果有则也删除外键在子表（即包含外键的表）中的记录

-   <b>set null：</b>当在父表（即外键的来源表）中删除对应的记录时，首先检查是否有对应的外键，如果有则设置子表中该外键值为 null（不过这就要求该外键允许为空）

### ON UPDATE

-   <b>restrict(约束)：</b>当在父表（即外键的来源表）中更新对应记录时，首先检查是否有对应的外键，如果有则不允许更新

-   <b>no action：</b>意思同 restrict

-   <b>cascade(级联)：</b>当在父表（即外键的来源表）中更新对应记录时，首先检查是否有对应的外键，如果有则也更新外键在子表（即包含外键的表）中的记录

-   <b>set null：</b>当在父表（即外键的来源表）中更新对应记录时，首先检查是否有对应的外键，如果有则设置子表中该外键值为 null（不过这就要求该外键允许为空）

> 注：`NO ACTION` 和 `RESTRICT` 的区别：只有在及个别的情况下会导致区别，前者是在其他约束的动作之后执行，后者具有最高的优先权执行。
