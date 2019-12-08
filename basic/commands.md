# 指令格式基础

就如物理公式那样，$P=\frac{W}{t}$。如果我们要求功率 $P$，那么就要将 $W$、$t$ 的值代入该公式。指令也是这样。如果我们要完成指定的目的，就要把指定的值代入到指定的位置。接下来，我们来学习**指令格式**（command pattern），这也是 OP 所要掌握的基础知识。在后文中，均会采用在本文中所使用的格式进行叙述。

## 指令的基本构成

Minecraft 中，指令以斜杠 `/` 开头，后跟指令的名称，例如 `/tp`、`/help`、`/kill` 等。为了规范，我们会统一将斜杠后的一部分内容称为**指令名**（command name）。指令名通常表示这个指令的用处，例如 `/tp` 指令的 `tp` 指的是 Teleport（传送）、`/help` 指令的 `help` 指的就是 Help（帮助），一目了然。

一个指令中，开头的 `/` 和指令名一起的一部分被称为**指令头**（command header），例如

```minecraft
/res set fairyhouse animalkilling false
```

中，`/res` 就是指令头。

那么，为了使一个指令有意义，我们必须在指令头后面跟上些什么，来表达我们想让这个指令实现的功能。就如上面那个指令一样，后面的 `set`、`fairyhouse` 等都是这个指令的**参数**（argument）。**一个指令必须有参数才有意义。**在我们指定了参数以后，如果正确，指令便会按照我们的想法去做了。

?> 一个指令的参数是以空格分隔的。例如上面所举的例子里面一共有 4 个参数，分别是 `set`、`fairyhouse`、`animalkilling` 和 `false`。指令名不算参数。

当然，也有特殊的存在。有的指令并不需要参数也可以正常执行，例如 `/help`。单独执行一句 `/help` 是不会出错的，照样会显示服务器里的帮助信息。不过，`/help` 指令仍然有参数——页码。所以我们也可以执行 `/help 2` 来查看第二个帮助页面，以此类推。

对于这种，如果一个参数实际存在，但是可带可不带，那么我们就把这种参数称为**可选参数**（optional argument）。可选参数存在的基础是默认行为，这个会在以后的内容中纵深讲述。除此之外，还有**无参指令**（orphan command），例如 Essentials（插件）的 `/plugins` 用来查看服务器已经安装的插件列表，它没有任何参数，而且也没必要有参数。

## 指令成功与错误

只要指令的内容表达正确，指令便能够成功执行。而指令的内容表达正确，仅需满足以下两个条件：

- 对于规定了的参数（例如某一参数只能填 1、2 或者 3，填 4 就是错的）没有违背规定；
- 填写了所有必填的参数。

也就是说满足以上两种条件即可成功执行指令。

**判断一个指令的成功与否，并不在于其是否实现了操作者的目的。**一个指令的成功与否应该取决于它执行后所输出的信息，如果没有任何报错，那么就可以说是执行成功。

例如如果你想要传送到玩家 Sapherise 的地方，你预想中是要输入 `/tp Sapherise`，但是*因为某种笔者也难以想象出来的原因* **打错**打成了 `/tp Subilan` 传送到了另一个玩家 Subilan 的地方，那么此时**指令也是成功执行的**。

然而当一个指令在执行以后任何反应也没有（既没有提示成功也没有提示失败），或者直接提示失败，又或者与预料结果偏差太大（例如执行 `/tp Subilan` 结果杀掉了 Subilan），我们就说这个指令**执行失败**了。

## 指令表达式

为了更科学地表达指令，我们应该学会阅读**指令表达式**（command expression）。以下是在指令表达式中常用的一些符号及其解释，当你记住以后便可很快理解一个指令的大致用法。

|       表达式        |   含义   |
| :-----------------: | :------: |
|     `/command`      |  指令头  |
| `[arg]` 或 `<arg?>` | 可选参数 |
|       `<arg>`       | 必选参数 |
|      `<a/b/c>`*      | 选择参数 |

**注：**大部分情况下我们会使用 `|` 符号而非 `/`。

为了帮助理解，我们可以看几个例子：

```minecraft
/kill <player-name?>
```

这代表了 `/kill` 指令的用法。我们可以从这个表达式中解读出下面的内容：

- 该指令包含一个参数 `player-name`，且该参数为选填的；
- 没有了。

又如 `/tp` 指令的用法可以这样表达

```minecraft
/tp <player-A> <player-B?>
```

- `/tp` 指令包含两个参数 `player-A` 和 `player-B`。其中 `player-B` 选填参数；
- 没有了。

这和我们对 `/tp` 指令的认知是符合的。如果 `/tp` 不跟任何参数，那么会执行失败——因此 `player-A` 是必填的；如果 `/tp` 后面只跟一个参数，则是传送到指令玩家的位置，如果跟两个参数，则是将第一个玩家传送到第二个玩家的位置。所以 `player-B` 是选填的。

上面的例子中，`player-name`、`player-A`、`player-B` 都是参数的名字，称为**参数名**（argument name）。参数名的用途是让人能够读懂这个参数的位置该填什么类型的信息。再比如上文中的 `/help` 指令，它的后面可以跟页码，所以 `/help` 指令的表达式就是这样写的：

```minecraft
/help <pagecode?>
```

更进阶地，我们可以使用计算机语言中对数据类型的表达方式来表示各个参数的类型。比如

```minecraft
# 固定类型参数
/tp <player-A: string> [player-B: string]
/help [pagecode: number]

# 非固定类型参数
/msg <player-name: string> [msg: any]
/me [msg]
```

当然，这可能会带来阅读困难且需要一定的计算机基础，所以在这里也只是提一下，在本教程中并不会使用。

接下来我们来做几个练习题：

1. **下列表示<u>可选参数</u>的是**

    A. `<arg>`
    B. `<arg?>`
    C. `[arg?]`
    D. `arg?`

2. **下列指令中，必须只带一个参数的是**

    A. `/op <玩家名>`
    B. `/tp <玩家名> <玩家名?>`
    C. `/kill [玩家名]`
    D. `/stop`

3. **（多选）下列指令中，能够执行成功的有**

    A. `/fly`
    B. `/gamemode`
    C. `/back`
    D. `/login`

<div class="reverse"><strong>答案:</strong> B A AC</div>
<style>
.reverse {
    transform: rotate(180deg);
    -ms-transform:rotate(180deg);
    -moz-transform:rotate(180deg);
    -webkit-transform:rotate(180deg);
    -o-transform:rotate(180deg);
    filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=1);
}
</style>

## 指令表达式例

为了更进一步的帮助理解，我们还是放出了许多指令的表达式并配上了相应的解说内容。下面的内容不需要特意掌握，并不会对指令的理解产生负面影响。

### 区间可选

```minecraft
/gamemode <player-name?> <mode-name>
```

`/gamemode` 指令用来切换游戏模式。

该指令的第一个参数是选填的——似乎违背常理，但是就是这样的。也就是说，如果我们直接在 `/gamemode` 后面填写模式名称，等同于我们直接填写了 `mode-name` 而把第一个 `player-name` 参数给留空了；反之，如果我们在 `/gamemode` 后面填写玩家名称，则代表我们填写了 `player-name` 这个参数，接下来要填写的是 `mode-name`。

这就好比

```minecraft
/gamemode creative
/gamemode Sapherise creative
```

第一个指令留空了 `player-name`，`creative` 这个值对应的是 `mode-name` 这个参数（虽然从位置上看貌似是对应着 `player-name`）。第二个指令把两个参数都填写了。

我们把这种情况称为**区间可选**（the optional between required）。

<h3>超<ruby>重<rt>chóng</rt></ruby>选择</h3>

```minecraft
/res <set|pset|create|auto|list|padd|pdel|info|setall|setallfor|message|limit|rc|...> ...
```

常常会有一些插件（例如 Residence）会将功能对应到各个参数，这就导致一个选择参数中包含了太多个选择，且每一个选择对应的后继参数是不一样的。比如 `/res set` 后面跟的是 `resname`、`flag`，而 `/res pset` 后面跟的是 `resname`、`player-name` 和 `flag`。这样表达起来会非常复杂。

所以对于这种情况，我们往往会将一个指令分成多个**子指令**（child command）：

```minecraft
/res:
set <resname> <flag> <status>
pset <resname> <player-name> <flag> <status>
create <resname>
auto ...
...
```

它们以同一个指令头开头，所以可以使用指令头+冒号的形式表示后面的所有指令均为其子指令，使用时前面需要加上这个指令头。