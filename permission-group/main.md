# 概述

<figure>
<img src="../assets/image/banner-cpgmp.png"/>
<figcaption>本章节封面</figcaption>
</figure>

**权限组管理模式**（Common Permission Group Management Pattern）是指依照不同的插件的不同权限接口设定对不同玩家类型的不同权限组的一种管理模式。大概可以理解为一种差异对待模式，或者通俗地理解成「不同类型的玩家有不同的权限」。

一般对于「不同类型的玩家」，各种服务器会给予自己的定义。例如**普通玩家**、**管理员**、**建筑师**、**VIP** 等等，都属于玩家的种类。权限组管理模式正是针对不同种类的玩家分别设定符合其身份的权限，以实现服务器游戏内容的基础规划。

## 插件基础

目前市面上较为流行的权限组管理插件包括 GroupManager（Essentials 系列）、LuckPerms 等。在本手册中将以 LuckPerms 为例进行展开。

这些插件可以用来轻松地管理服务器的权限组。没有这些插件是无法进行权限组管理的，因此我们称这些插件为权限组管理模式的基础。