# 1. 自定义功能

> 本页面的所有内容都是完全可选的，不计入分数！！
> 如果你想为自己的游戏版本添加一些很酷的自定义内容，这些内容会对你有帮助。

* [1. 自定义功能](#1-customisations)

  * [1.1. 影响游戏外观和控制渲染](#11-influencing-the-look-of-the-game-and-controlling-rendering)
  * [1.2. 本地化/更改文本](#12-localisationchanging-text)
  * [1.3. 渲染一个非常简单的世界](#13-getting-the-map-to-render-a-very-simple-world)

    * [1.3.1. 其他一些说明](#131-some-other-comments-about-this)
  * [1.4. 参考](#14-reference)
  * [1.5. 更改图形和样式](#15-changing-graphics-and-styling)

    * [1.5.1. 图块集](#151-tilesets)
  * [1.6. 目标](#16-goals)

    * [1.6.1. 如何标志游戏结束](#161-how-to-signal-that-the-game-has-finished)
  * [1.7. 定义动画和音效提示](#17-defining-animations--audio-cues)

    * [1.7.1. 通过皮肤文件](#171-through-the-skin-file)
  * [1.8. 随机音频](#18-randomising-audio)
  * [1.9. 动画](#19-animations)

    * [1.9.1. 简单的生命条](#191-a-simple-health-bar)
    * [1.9.2. 更疯狂的例子！](#192-a-more-crazy-example)
    * [1.9.3. 更详细的规范](#193-a-more-detailed-specification)
    * [1.9.4. 所有可用的动作](#194-all-possible-actions)

## 1.1. 影响游戏外观和控制渲染

学习如何与一个已经构建好（可能不太优雅）的系统交互，是设计体验的重要组成部分，这是我们希望在本课程中鼓励的技能。

## 1.2. 本地化/更改文本

我们从一个非常简单的例子开始，我们将把你的名字添加到主菜单中，并更改一张图片 \:D。

首先，请确保你的项目完全设置好了。接着我们运行该项目。虽然你还不能做什么炫酷的事情（因为你还没有写代码），但通过伪造一个世界状态，至少我们可以看到系统是如何运作的。

运行项目时，你会看到如下界面：

![](/images/customisations1.png)

我们来更改第二个子标题（写着 `Backend created by (your names here)` 的那一行）。怎么改？其实非常简单！你甚至不需要重新运行程序。

你只需要找到一个名为 `src/main/resources/languages/en_US.json` 的文件（目前我们还没有语言选择器，但这可能是未来会实现的功能）。我们已经为你复制了该文件的部分内容，供你参考：

```json
{
    "main_menu": {
        "font": "alagard.ttf",
        "title": "Dungeon Mania",
        "subtitle_1": "Frontend created by:\n Braedon Wooding, George Litsas, Nick Patrikeos, Noa Challis",
        "subtitle_2": "Backend created by: <your names here>",
        ...
    },
    "game": {
        ...
    }
}
```

> 为简洁起见，这里使用 `...` 表示文件中还有其他不相关内容。

你会注意到，我们可以在这里做很多事情，例如更换字体、更改标题，甚至几乎修改游戏中所有文本！我们现在来修改 `subtitle_2`，不用添加你们整个小组的名字，只需要添加你自己的（注意避免合并冲突，因此可以更有协调地进行这项更改）。然后刷新页面，瞧！它已经更新啦！

## 1.3. 渲染一个非常简单的世界

接下来我们将手动构建一个非常简单的世界。虽然我们还不能使用方向键（因为我们还没处理 tick 中的移动逻辑），但我们至少能加载一个简单的世界。

先看下图所示的世界，找出其中玩家与周围区域的细节，他们被墙壁包围，旁边有一扇门。

![](/images/customisations2.png)

生成这个世界的代码如下，应放置在 `newGame` 方法中。以下代码较为粗略，仅作演示用途。

```java
public DungeonResponse newGame(String dungeonName, String gameMode) throws IllegalArgumentException {
    List<EntityResponse> entities = new ArrayList<>();

    // 生成一些墙壁
    /**
        *
        *  WWWWW
        *  W P W
        *  W D W
        *  WWWWW
        *
        * P 是玩家，D 是门，W 是墙壁
        */

    for (int x = 0; x < 5; x++) {
        for (int y = 0; y < 4; y++) {
            if (x == 0 || y == 0 || x == 4 || y == 3) {
                // 只生成边框
                entities.add(new EntityResponse("entity-" + x + " - " + y, "wall", new Position(x, y, 2), false));
            }
        }
    }

    // 添加玩家和门
    entities.add(new EntityResponse("entity-player", "player", new Position(2, 1, 2), false));
    entities.add(new EntityResponse("entity-door", "door", new Position(2, 2, 2), false));

    return new DungeonResponse("some-random-id", dungeonName, entities, Arrays.asList(new ItemResponse("item-1", "bow"), new ItemResponse("item-2", "sword")), new ArrayList<>(), "");
}
```

但是，当你运行这个（记得在代码修改后重启服务器），你会发现门并没有渲染出来，看到的是一个奇怪的绿色矩形？为什么？这是因为找不到门的图像资源！

![](/images/customisations3.png)

我们需要手动添加这个资源。进入 `src/main/resources/skins/default.json` 文件，你会看到如下内容：

```json
{
  "main_menu": {
    "background_image": "images/backgrounds/main_menu.png",
    "text_color": "white"
  },
  "game": {
    "background_image": {
      "images/tileset/walls/wooden/wood_floor_0.png": 5,
      "images/tileset/walls/wooden/wood_floor_1.png": 7,
      "images/tileset/walls/wooden/wood_floor_2.png": 10,
      "images/tileset/walls/wooden/wood_floor_3.png": 2
    },
    "resolution_px": 16,
    "item_box": "images/ui/box.png",
    "text_color": "white"
  },
  "entities": {
    "boulder": "images/tileset/entities/boulder_00.png",
    "bow": "images/tileset/items/bow_0.png",
    "player": "images/tileset/entities/entity.png",
    "bomb": "images/tileset/items/bomb.png"
    // ... （还有很多实体）
  }
}
```

小提示：你可以修改 `resolution_px` 来控制图像渲染大小。如果你希望渲染高分辨率图片，这个参数非常有用。

我们只需在 `entities` 部分添加一行，如下所示（别忘了给上一行加逗号）：

```diff
    "entities": {
        "boulder": "images/tileset/entities/boulder_00.png",
        "bow": "images/tileset/items/bow_0.png",
        "player": "images/tileset/entities/entity.png",
        "bomb": "images/tileset/items/bomb.png",
        "wood": "images/tileset/entities/box_wood.png",
        "arrow": "images/tileset/items/arrow_0.png",
        "exit": "images/tileset/entities/stairs_down.png",
        "treasure": "images/tileset/items/treasure_0.png",
        "wall": "images/tileset/walls/wooden/wood_bottom.png",
        "spider": "images/tileset/entities/spider_00.png",
        "mercenary": "images/tileset/entities/ranger.png",
        "switch": "images/tileset/floor_switch.png",
        "invincibility_potion": "images/tileset/items/potion_10.png",
-       "sword": "images/tileset/items/sword_silver.png"
+       "sword": "images/tileset/items/sword_silver.png",
+       "door": "images/tileset/walls/wooden/door_locked_silver.png"
    }
```

注意 `"sword"` 那行后面的逗号。我们为你准备了大量图片素材（大部分已经命名和分类，有些留给你自己命名分类）。你也可以使用自己找到的图像资源（我们甚至鼓励你这样做！）。

修改完后，**不需要重启程序**！

![](/images/customisations4.png)

你也可以尝试更改这个例子，比如改为渲染一个宝藏。查看各种实体类型，看看能找到什么资源。最后你应该得到类似这样的效果：

![](/images/customisations5.png)

至此，你已经能创建一个简单世界，并修改图像资源了。还有更多内容值得探索，比如图块集等。

### 1.3.1. 其他一些说明

你可能已经注意到，前端其实并不知道什么是“实体”，它只知道你想渲染一张图片。此外，你可能对 `Position` 的第三个参数有疑问，它被称为 `layer`，用于控制实体之间的层叠关系。

更深入一点讲，我们可以引入“Z轴排序（z sorting）”的概念，即决定哪个实体/图像应该显示在其他实体之上或之下。我们通过第三个坐标 `z` 或称 `layer` 来暴露这个功能。例如位置 `(1, 1, 0)` 表示在 `x=1, y=1` 且层级为 0，会在 `(1, 1, 1)` 之下显示。我们建议你规范使用层级（比如所有物品都放在同一层），而不是随意设定。实体是可以在层之间移动的。

## 1.4. 参考

该项目为你提供了极大的灵活性，允许你根据自己的需求自定义游戏的视觉效果、音效和机制。唯一需要注意的是：你不应简化解决方案的复杂性（即你可以进行横向的修改，但不能简化某项机制的实现方式）。

为了帮助你实现这些，前端是非常灵活的，支持你定义以下内容：

* 为每个图形指定不同的图片
* 样式（颜色、字体等）
* 事件触发时的动画与音效提示

该系统并非为了完美而设计，你可能需要绕过某些尴尬的设计决策（就像现实开发中那样），从而提出一个干净简洁的解决方案。

## 1.5. 更改图形和样式

若你想启用替代图形，需要设置一个名为 "skin.json" 的文件；这些 JSON 文件允许你自定义图形。例如：

```json
{
  "main_menu": {
    "background_image": "images/MyFile.jpg",
    "text_color": "rgb(255, 0, 0)"
  }
}
```

默认皮肤文件（`src/resources/skins/default.json`）会展示你可以更改的所有内容。注意：实体（如图块、角色、物品等）不会出现在皮肤文件中，因为它们的图标是在实体本身中指定的。

### 1.5.1. 图块集

有时你可能希望基于某种权重随机图像，你可以通过图块集来实现。从默认资源文件中：

```json
        "background_image": {
            "images/tileset/walls/wooden/wood_floor_0.png": 5,
            "images/tileset/walls/wooden/wood_floor_1.png": 7,
            "images/tileset/walls/wooden/wood_floor_2.png": 10,
            "images/tileset/walls/wooden/wood_floor_3.png": 2
        },
```

你可以把这些数字理解为“抽奖券”的数量。当我们选择要渲染的图像时，抽奖券越多的图像越容易被选中。这种方式不是基于百分比，因此更容易扩展并保持友好。

## 1.6. 目标

你可能希望在某个阶段正确地渲染目标内容，就像下面这样：

![](/images/customisations6.png)

其实比你想的要简单得多！上面的例子实际上就是 `":treasure AND :boulder"`（你可以在 dungeon response 中硬编码它并看看效果！）。原理是：任何在目标字符串中以冒号 `:` 开头的文本都会被解析为图像，而不是纯文本。这些图像通常对应实体类型。你还可以做一些比较花哨的写法，比如 `":treasure(5) AND :boulder(4)/:switch(3)"`（表示你需要收集5个宝藏、压住3个开关并且需要4个石头）。

![](/images/customisations7.png)

### 1.6.1. 如何标志游戏结束

当你希望游戏结束时，只需返回一个空字符串作为 goals。**前提是你之前给该游戏设置过目标**，这样它才会触发结束游戏的提示弹窗。

## 1.7. 定义动画和音效提示

你可以通过两种方式定义在特定事件中播放的动画/音效提示：一种是被动式（reactive），另一种是主动式（proactive）。

### 1.7.1. 通过皮肤文件

> 关于音乐的几点限制：当浏览器标签页不活跃时音乐不会播放；如果你使用 Chromium 浏览器，必须先点击游戏画面，音乐才会播放（这是 Chrome 的限制）；左下角有一个静音按钮；此外，VLAB 不支持音频。

你可以在皮肤文件中指定背景音乐，例如：

```json
{
    "main_menu": {
        "background_music": "audio/myMusic.mp3"
        ...
    },
    "game": {
        "background_music": "audio/otherMusic.mp3"
        ...
    }
}
```

系统支持 mp3 和 ogg 格式。请注意：在 Firefox 中 mp3 可能无法正常播放，因此你可能需要将其转换为 ogg。

> 实际上，文件夹的名称无所谓（只要文件在 `src/resources` 目录下即可，路径更多是出于风格组织的目的）。

默认皮肤文件（`src/resources/skins/default.json`）会展示你可以修改的大多数事件项，例如点击按钮、背景音乐等。

## 1.8. 随机音频

你可以像使用图块集那样配置多个音频资源来实现随机播放，例如：

```json
{
  "main_menu": {
    "background_music": {
      "audio/a.mp3": 3,
      "audio/b.mp4": 10
    }
  }
}
```

一首随机选中的曲目播放完毕后，系统将自动选择另一首播放。不会连续播放相同的音轨（除非你只配置了一首）。

## 1.9. 动画

动画是现代应用程序中的重要组成部分，虽然前端对动画的支持有限，但仍可以实现一些功能。动画绑定在实体上，在指定的帧中持续存在。

### 1.9.1. 简单的生命条

以下是一个生成简单生命条的例子：

```java
public DungeonResponse newGame(String dungeonName, String gameMode) throws IllegalArgumentException {
        List<EntityResponse> entities = new ArrayList<>();

        // 生成墙壁（略）

        // 添加玩家实体
        entities.add(new EntityResponse("entity-player", "player", new Position(2, 1, 2), false));

        List<AnimationQueue> animations = new ArrayList<>();
        // 参数说明：
        // - when（当前未使用，设为 "PostTick"）
        // - entityId（要应用动画的实体 ID）
        // - queue（动作队列）
        // - loop（是否循环）
        // - duration（动画持续时间，负数表示无限）

        animations.add(new AnimationQueue("PostTick", "entity-player", Arrays.asList(
            "healthbar set 0.8", "healthbar tint 0x00ff00", "healthbar set 0.2, over 1.5s", "healthbar tint 0xff0000, over 0.5s"
        ), false, -1));

        animations.add(new AnimationQueue("PostTick", "entity-player", Arrays.asList("healthbar shake, over 0.5s, ease Sin"), false, 0.5));

        return new DungeonResponse("some-random-id", dungeonName, entities, Arrays.asList(new ItemResponse("item-1", "bow"), new ItemResponse("item-2", "sword")), new ArrayList<>(), "", animations);
}
```

\*\*注意：\*\*你目前是每次都重新创建动画，思考下如何将其抽象化处理。

渲染结果如下：

![](/images/customisations8.png)

### 1.9.2. 更疯狂的例子！

将上面的动画创建代码替换为以下内容。在这个例子中，我们保留了生命条动画并让其循环播放 10 秒，然后将玩家的精灵图替换为石块，并让其沿 x 轴缓慢移动。

```java
animations.add(new AnimationQueue("PostTick", "entity-player", Arrays.asList(
    "healthbar set 0.8", "healthbar tint 0x00ff00", "healthbar set 0.2, over 1.5s", "healthbar tint 0xff0000, over 0.5s"
), true, 10));

animations.add(new AnimationQueue("PostTick", "entity-player", Arrays.asList(
    "healthbar shake, over 0.5s, ease Sin", "tint 0x00ff00", "translate-x 1, over 2s", "sprite boulder"
), false, 0.5));
```

> ⚠ 注意：如果你在处理移动逻辑，建议在 `EntityResponse` 中返回最终位置，然后通过一个持续时间为 0 的平移先移回旧位置，再从旧位置平移回新位置。即：`translate-x -1, over 0s` 后跟 `translate-x 1, over 2s`

### 1.9.3. 更详细的规范

动画队列（AnimationQueue）是一个非常简单的类，定义了一组可以对实体执行的动作。其结构如下：

```java
public class AnimationQueue {
    /** 何时播放动画？当前未使用，设为 "PostTick" */
    private final String when;

    /** 要应用动画的实体 ID */
    private final String entityId;

    /** 要执行的动作列表（详见下方） */
    private final List<String> queue;

    /** 如果为 true，动画将在队列全部完成后循环播放 */
    private final boolean loop;

    /** 如果启用了 loop，则动画会重复播放直到累计时间达到该值 */
    private final double duration;
}
```

构造函数如下：

```java
public AnimationQueue(String when, String entityId, List<String> queue, boolean loop, double duration);
```

此外，`DungeonResponse` 也提供了一个重载版本，可以接收动画队列：

```java
public DungeonResponse(String dungeonId, String dungeonName, List<EntityResponse> entities,
        List<ItemResponse> inventory, List<ItemResponse> buildables, String goals,
        List<AnimationQueue> animations)
```

### 1.9.4. 所有可用的动作

你通常只需使用 `entityId` 和 `queue` 来定义专属动画。

队列中的动作通过空格和逗号分隔参数，因此请确保格式完全匹配。动作是通过插值实现的（即从旧状态平滑过渡到新状态）。

以下是所有适用于生命条的命令：

* `healthbar`：表示作用于生命条的动画指令前缀
* `healthbar set (amount)`：设置生命条进度，取值范围 0 <= amount <= 1
* `healthbar shake`：使生命条抖动
* `healthbar scale (amount)`：将生命条缩放为指定比例
* `healthbar tint (color)`：将生命条颜色设置为指定的十六进制颜色码，例如 `0xff00ff`

以下是所有通用实体支持的命令：

* `sprite (entity-type)`：重写所用精灵图（忽略 `ease` 和 `over` 修饰符），图像路径从 `default.json` 中的实体映射中查找
* `tint (hex)`：将精灵图染色为指定的十六进制颜色
* `rotate (degrees)`：将图像旋转指定角度
* `scale (ratio)`：将实体整体缩放为指定倍数
* `scale-x (ratio)`：设置实体在水平方向的缩放比例
* `scale-y (ratio)`：设置实体在垂直方向的缩放比例
* `translate-x (x)`：沿 x 轴平移实体，单位比例与图块大小一致（`translate-x 1 == 一个图块`）
* `translate-y (x)`：沿 y 轴平移实体
* `set-layer (layer)`：设置实体的图层编号，用于控制其显示在其他实体的上/下方
