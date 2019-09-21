# LuckPerms 常用指令

LuckPerms 是一款开源的 Minecraft 插件，用于管理服务器的权限组。Sotap 使用的就是 LuckPerms。LuckPerms 支持 Bukkit、Spigot、Sponge 等多平台。

在这里，我们会介绍少许几个在 LuckPerms 中常常使用的指令。

## 基本结构

LuckPerms 的指令是有结构的。如下：

```minecraft
lp <object-type> <object-name> <controller> <action> <value> <context?>
```

乍一看会很陌生。我们来使用表格一一解释。

|英文名称|中文名称|含义|
|:-:|:-:|:-:|
|`object-type`|对象种类|当前指令所操作的对象的种类，例如要操作一个**组**，那么该项的值为 `group`；如果要操作一个**玩家**，那么该项的值为 `player`|
|`object-name`|对象名称|当前指令所操作的对象的名称，直接填写即可|
|`controller`|控制器|即所要操控的类型。例如要更改权限，那么该值为 `permission`；如果要更改父级组，那么该值为 `parent`，等等|
|`action`|操作|即要进行的操作。例如要进行「设置」操作，那么该值为 `set`|
|`value`|值|要将该项所设定到的值|
|`context`|上下文|限定该指令所设置的内容影响的范围，例如仅在名为 `abc` 的世界生效等|
