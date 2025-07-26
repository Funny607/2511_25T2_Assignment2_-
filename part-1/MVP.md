# 1. 前言与问题

多年来，Penguin Pty Ltd（由 Atilla Brungs 运营的一家小型软件公司）凭借热门游戏在原生应用游戏市场中占据主导地位。然而，近年来，Web 技术的进步使得新一代消费者不再愿意下载并运行原生应用，而是希望在线玩游戏。为了适应不断变化的市场，Penguin Pty Ltd 在 2021 年决定带用户回到 20 世纪 80 年代，开发一款名为 Dungeonmania 的新游戏，并加入现代元素——该游戏被设计为 Web 应用程序。

他们的工程团队共同构建了一个 MVP（最简可行产品），让 Penguin 再次登上排行榜榜首——但现在用户想要更多！努力工作的后端工程师，也就是上个学期的 COMP2511 学生们都已经离开。销售低迷导致现在只能保留两位员工来负责后端，而非之前的五人团队。更糟糕的是，前任工程师在实现中留下了一系列设计问题。

\[\[*TOC*]]

# 2. 产品规格（MVP）

你和你的搭档被雇佣并继承了 Dungeonmania 游戏的现有代码库。

你们获得了 Dungeonmania MVP 版本的产品规格说明，以帮助你理解现有代码及其所提供的功能。

> ***注意：*** 本文件中的所有功能已经在我们提供给你的 monolith 仓库中实现。你不需要自行实现它们。

在 Dungeon Mania 中，你控制一名玩家，并需要在一系列地下城中完成各种目标以通关游戏！

![](/images/dungeon1.png)

这种谜题的最简单形式是迷宫，玩家必须从起点找到通往出口的路径。

![](/images/dungeon2.png)

更高级的谜题可能包含需要推到地面开关上的巨石，

![](/images/dungeon3.png)

需要用武器战斗的敌人，或像药水和宝藏这样的可收集物品。

![](/images/dungeon4.png)

## 2.0 地图

实体占据地图上的格子。每个格子有一个 (x,y) 坐标。

有关地图技术细节的更多信息请参阅 [第 4.1 节](#41-dungeon-maps)。

## 2.1 玩家

每局游戏中只有一个玩家。

玩家可以向上、下、左、右移动到相邻的格子，前提是没有其他实体阻挡（例如墙）。玩家在游戏开始时拥有一定的生命值和攻击力。玩家从一个设定好的“入口位置”开始生成。

## 2.2 静态实体

游戏包含以下静态实体。

<table>
<thead>
  <tr>
    <th><span style="font-weight:bold">实体</span></th>
    <th><span style="font-weight:bold">图片</span></th>
    <th><span style="font-weight:bold">描述</span></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>墙</td>
    <td><img src='/images/wall.png' /></td>
    <td>阻挡玩家、敌人和巨石的移动。</td>
  </tr>
  <tr>
    <td>出口</td>
    <td><img src='/images/exit.png' /></td>
    <td>如果玩家穿过它，则可能完成该谜题。</td>
  </tr>
  <tr>
    <td>巨石</td>
    <td><img src='/images/boulder.png' /></td>
    <td>在大多数情况下表现得像墙。唯一的区别是它可以被玩家推入相邻格子。玩家一次只能推动<strong>一个</strong>巨石。当玩家推动巨石时，他们会移动到巨石原本所在的位置。巨石可以被推到可收集实体上。</td>
  </tr>
  <tr>
    <td>地面开关</td>
    <td><img src='/images/switch.png' /></td>
    <td>开关的表现与空格子类似，因此其他实体可以出现在其上。当一个巨石被推到地面开关上时，开关会被触发。将巨石从开关上移开会取消触发状态。</td>
  </tr>
  <tr>
    <td>门</td>
    <td><img src='/images/door.png' /></td>
    <td>与一个可以打开它的钥匙一起存在。如果玩家持有钥匙，他们可以通过移动穿过门来打开它。一旦门被打开，将保持开启状态。</td>
  </tr>
  <tr>
    <td>传送门</td>
    <td><img src='/images/portal.png' /></td>
    <td>将玩家传送到对应的传送门。玩家必须最终落在对应传送门相邻的格子上。传送到的格子也必须符合移动限制——例如，玩家不能传送到墙上。如果所有相邻格子都是墙，则玩家应保持原位不动。</td>
  </tr>
  <tr>
    <td>僵尸吐司生成器</td>
    <td><img src='/images/zombie_spawner.png' /></td>
    <td>在与生成器相邻的空格子中生成僵尸吐司。如果相邻的格子都不为空，则不会生成僵尸。如果玩家有武器（剑或弓）并靠近生成器，他们可以与生成器交互将其摧毁。生成器将立即被摧毁，武器的耐久度减少 1。</td>
  </tr>
</tbody>
</table>

## 2.3 移动实体

除了玩家外，游戏还包含以下可移动实体。

所有敌对实体都可以作为初始地下城的一部分生成。每个时间单位（tick），所有敌人都会根据各自的行为规则移动。

<table>
<thead>
  <tr>
    <th><span style="font-weight:bold">实体</span></th>
    <th><span style="font-weight:bold">图片</span></th>
    <th><span style="font-weight:bold">描述</span></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>蜘蛛</td>
    <td><img src='/images/spider.png' /></td>
    <td>蜘蛛在游戏开始时随机生成在地下城中的位置。生成后，它们会立即向上移动 1 格（即朝屏幕顶部），然后开始围绕其出生点“旋转”（见下方示意图）。<br>
    <details margin-top="10px">
        <br>
        <summary>蜘蛛移动图示 1</summary>
        <br>
        <img src='/images/spider_movement1.png' width=200px>
        </details>
    <br>
    蜘蛛可以穿越墙、门、开关、传送门和出口（这些不会对其产生影响），但不能穿越巨石，若遇到巨石将会反向移动（见下方图示）。
    <details>
        <summary>蜘蛛移动图示 2</summary>
        <br>
        <img src='/images/spider_movement2.png' width=200px>
        </details>
    <br>
    蜘蛛会生成在玩家当前位置周围曼哈顿距离小于等于 20 的格子内。如果没有可用空间，则不会生成蜘蛛。蜘蛛不能生成在巨石上，或与玩家/敌人重叠的位置。如果蜘蛛在其移动路径中被两个巨石夹住，它将保持静止。
</td>
  </tr>
  <tr>
    <td>僵尸吐司</td>
    <td><img src='/images/zombie.png' /></td>
    <td>僵尸从僵尸生成器生成，并向随机方向移动。它们受到与玩家相同的移动限制，但传送门对其无效。</td>
  </tr>
  <tr>
    <td>佣兵</td>
    <td><img src='/images/mercenary.png' /></td>
    <td>佣兵不会自行生成；只有在地下城中预置时才会出现。它们会持续向玩家移动，仅当无法靠近玩家时才会停止（例如会绕过墙）。佣兵受到与玩家相同的移动限制。所有佣兵默认是敌对的，除非玩家使用一定数量的金币贿赂他们；此时他们会成为盟友。佣兵必须在一定半径范围内才能被贿赂，这个范围是斜对角和正对角格子构成的“方形”区域，类似炸弹的爆炸半径。作为盟友后，一旦靠近玩家，他们会持续跟随玩家，并站在玩家原本所在的格子。</td>
  </tr>
</tbody>
</table>

## 2.4 可收集实体

玩家通过走到物品所在的格子上可以拾取可收集实体。拾取后，该物品将被放入玩家的背包中。

<table>
<thead>
  <tr>
    <th><span style="font-weight:bold">实体</span></th>
    <th><span style="font-weight:bold">图片</span></th>
    <th><span style="font-weight:bold">描述</span></th>
  </tr>
</thead>
<tbody>
    <tr>
    <td>宝藏</td>
    <td><img src='/images/treasure.png' /></td>
    <td>可以被玩家拾取。</td>
  </tr>
    <tr>
    <td>钥匙</td>
    <td><img src='/images/key.png' /></td>
    <td>当玩家进入该物品所在的格子时可拾取。玩家一次只能携带一把钥匙，且每把钥匙只对应一扇特定的门。一旦钥匙被使用（例如开门或合成物品），它就会消失。如果钥匙在开门之前被用于其他用途，对应的门可能会永远被锁住。</td>
  </tr>
    <tr>
    <td>无敌药水</td>
    <td><img src='/images/invincibility_potion.png' /></td>
    <td>玩家拾取无敌药水后，可以随时使用。只要无敌效果存在，战斗将在第一回合立即结束，玩家立刻胜利并且不受任何伤害。处于无敌状态时，僵尸和敌对佣兵会逃离玩家。蜘蛛和友军佣兵的移动不受影响。药水效果持续时间有限。</td>
  </tr>
  <tr>
    <td>隐身药水</td>
    <td><img src='/images/invisibility_potion.png' /></td>
    <td>玩家拾取隐身药水后，可以随时使用，使用后玩家立刻变为隐形，并可不被发现地穿过其他实体。此时敌对佣兵不再追击玩家，而是改为随机移动，但盟友仍会继续跟随玩家。隐身状态下不会发生战斗。</td>
  </tr>
  <tr>
    <td>木材</td>
    <td><img src='/images/wood.png' /></td>
    <td>可以被玩家拾取。</td>
  </tr>
  <tr>
    <td>箭</td>
    <td><img src='/images/arrows.png' /></td>
    <td>可以被玩家拾取。</td>
  </tr>
  <tr>
    <td>炸弹</td>
    <td><img src='/images/bomb.png' /></td>
    <td>可被玩家拾取。使用后从背包中移除并放置在当前所在地图位置。当炸弹与一个已激活的开关正对相邻时，它会爆炸，摧毁其对角线和正对角所有相邻格子中的所有实体（不包括玩家），形成一个“方形”爆炸半径。如果炸弹被放置在已激活的开关旁，或放置后相邻的开关被激活，则会立即引爆。炸弹一旦放置或爆炸后无法再次拾取。</td>
  </tr>
  <tr>
    <td>剑</td>
    <td><img src='/images/sword.png' /></td>
    <td>一种标准的近战武器。可以被玩家拾取并用于战斗，提升攻击力（加法）。每把剑有一定耐久度，限制它可以参与的战斗次数。耐久耗尽后不可再用。</td>
  </tr>
</tbody>
</table>

玩家同一时间只能处于一种药水效果之下。若玩家在药效持续期间再次使用药水（无论是否同种类），新药水效果不会立即生效，而是“排队”等待前一药效结束后一刻生效。例如：

* tick 0：玩家使用持续 5 个 tick 的隐身药水，当 tick 开始时立即变为隐形
* tick 3：玩家再次使用无敌药水
* tick 4 结束时（即敌人移动后），隐身效果结束，玩家变为可见且获得无敌效果

## 2.5 可建造实体

某些实体可以由玩家通过“配方”建造，即将多个实体组合成更复杂且更有用的实体。一旦建造完成，该物品会被放入玩家的背包中。对所有可建造实体而言，建造完成后所用材料会从玩家的背包中消失。

<table>
<thead>
  <tr>
    <th><span style="font-weight:bold">实体</span></th>
    <th><span style="font-weight:bold">图片</span></th>
    <th><span style="font-weight:bold">描述</span></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>弓</td>
    <td><img src='/images/bow.png' /></td>
    <td>可通过 1 个木材 + 3 个箭合成。弓有耐久度，使用次数有限。弓使玩家在每回合造成双倍伤害，用于模拟远程攻击（实际上无法远程攻击）。</td>
  </tr>
  <tr>
    <td>盾牌</td>
    <td><img src='/images/shield.png' /></td>
    <td>可通过 2 个木材 + （1 个宝藏 或 1 把钥匙）合成。盾牌可降低敌人攻击效果。每个盾牌有特定耐久度，决定可参与的战斗次数。</td>
  </tr>
</tbody>
</table>

## 2.6 战斗

当玩家与敌人出现在同一个格子时（在同一个 tick 内的任意时刻），将触发战斗。无论是玩家移动到敌人所在格子，还是敌人移动到玩家所在格子，战斗都会发生。

一轮战斗的计算方式如下：

```
玩家生命值 = 玩家生命值 -（敌人攻击力 / 10）
敌人生命值 = 敌人生命值 -（玩家攻击力 / 5）
```

每轮中，玩家和敌人同时受到伤害。

如果玩家生命值 <= 0，则玩家死亡并从游戏中移除，游戏结束。
如果敌人生命值 <= 0，则敌人死亡并从游戏中移除。如果在上述回合后，双方都未死亡，则战斗继续下一轮，直到有一方死亡。

所有战斗回合均在同一个 tick 中完成。

### 2.6.1 战斗中的武器

在战斗中，武器和盟友可为玩家提供攻击和防御加成。

这些增益可以是加法（+）、减法（-）、乘法（\*）或除法（/），并可叠加。玩家可以持有多个相同类型的武器/盟友。

以下是使用弓、剑和盾牌的战斗示例：

```
玩家生命值 = 10
玩家基础攻击力 = 5
弓攻击加成 = 2
剑攻击加成 = 1
盾牌防御 = 2
敌人生命值 = 10
敌人攻击力 = 5

战斗开始：
- 回合 1   敌人生命值 = 10 - ((2 * (5 + 1)) / 5)  = 7.6
            玩家生命值 = 10 - ((5 - 2) / 10)       = 9.7
- 回合 2   敌人生命值 = 7.6 - ((2 * (5 + 1)) / 5) = 5.2
            玩家生命值 = 9.7 - ((5 - 2) / 10)      = 9.4
- 回合 3   ...
```

所有加法/减法的加成在乘法/除法加成之前处理。

## 2.7 目标

除了地图布局外，每个地下城还包含一个目标，定义了玩家必须完成哪些条件才能通关该地下城。基本目标包括：

* 站在出口上；
* 所有地面开关上都有一个巨石；
* 收集一定数量（或更多）的宝藏。这里指的是累计收集数量，而非背包中当前持有数量。

目标只会在第一个 tick 之后开始评估。

目标可能会在达成后被“取消”，例如玩家到达出口（目标达成）后离开，或巨石从地面开关上被移开。

### 2.7.1 复合目标

更复杂的目标可以通过逻辑组合多个基本目标构建。例如：

* 收集一定数量的宝藏 且 到达出口；
* 收集一定数量的宝藏 或 所有地面开关上有巨石；
* 到达出口 且（所有地面开关上有巨石 或 收集所有宝藏）

所有复合目标都是二元的（即仅包含两个子目标）。

## 2.8 胜利与失败

当所有目标达成时，游戏获胜。
当玩家死亡并从地图中移除时，游戏失败。
游戏在胜利或失败后，tick 是否继续没有强制规定。

游戏也可能进入不可胜利状态，在这种情况下，游戏继续进行。

## 2.9 高级移动

佣兵的移动使用 Dijkstra（迪杰斯特拉）算法，以最短路径接近玩家。

<details>
   <summary>该算法伪代码</summary>
   <br>
   注意：这不一定是最优方案（A* 算法可能更适合我们的迷宫类地图），但因为本课程为设计课程而非算法课程，因此可以接受。

```
function Dijkstras(grid, source):
    let dist be a Map<Position, Double>
    let prev be a Map<Position, Position>

    for each Position p in grid:
        dist[p] := infinity
        previous[p] := null
    dist[source] := 0

    let queue be a Queue<Position> of every position in grid
    while queue is not empty:
        u := next node in queue with the smallest dist
        for each cardinal neighbour v of u:
            if dist[u] + cost(u, v) < dist[v]:
                dist[v] := dist[u] + cost(u, v)
                previous[v] := u
    return previous
```

</details>

> 📝 所有与“半径”相关的距离，除非特别说明，均指 [曼哈顿距离](https://iq.opengenus.org/manhattan-distance/)。

## 2.10 Tick 定义

Tick 表示从一个状态转换到另一个新状态。每个 tick 总是从用户输入开始。用户输入可以包括移动、使用物品、合成物品、或与实体交互。

然后游戏世界发生变化，该 tick 结束，等待下一次用户输入。因此，“tick n” 表示从第 n 个状态过渡到第 n+1 个状态。一个 tick 内可以包含多个开发者定义的“阶段”，以决定游戏世界变化的顺序。以下是阶段顺序的示例，帮助你理解。

![](images/tick.png)

在 tick 的某一阶段内，动作可以按任意顺序发生。例如，敌人可以按任意顺序移动。又如，如果玩家拾取物品并触发战斗，这两个动作的发生顺序是无关紧要的。

# 3. UML 图

本仓库中包含一个 [MVP 规格的 UML 示例图](MVP_UML.pdf)。

请注意，该图可能存在轻微不准确或遗漏，仅供参考。最终要求以文字规格为准。

你可以选择使用 [此 LucidChart 链接](https://lucid.app/lucidchart/6400884c-7337-4e46-978c-daee1c68c796/editNew?existing=1&docId=6400884c-7337-4e46-978c-daee1c68c796&shared=true&invitationId=inv_96df826d-2a71-4510-a0b7-0b2a115f3d7c&page=0_0#) 创建一个你个人可编辑的 UML 图副本，但不是必须。

# 4. 技术规格

## 4.1. 地下城地图

**所有地图在所有方向上都是无限的。**

向左/右移动分别表示实体的 x 坐标减少/增加，向上/下移动分别表示实体的 y 坐标减少/增加。

每个游戏都需要加载一个地下城地图。地下城地图由包含以下内容的 JSON 文件组成：

* `entities`：游戏开始时地图中的实体数组；
* `goal-condition`：用于描述通关地下城所需完成的目标。

JSON 文件中不会存在其他字段。

### 4.1.1 输入规格 - 实体（MVP）

entities JSON 数组中的每个条目都是一个 JSON 对象，包含以下字段：

* `x` - 游戏开始时实体在地下城中的 x 坐标；
* `y` - 游戏开始时实体在地下城中的 y 坐标；
* `type` - 实体的类型。

每个实体在地图上的 `x` 和 `y` 组合都是唯一的——即每个方块上最多只有一个实体。

`type` 字段是一个字符串，以下列前缀之一开头。为了自动评分，传入的所有实体类型都将符合下表中的类型。

<table>
<thead>
  <tr>
    <th>实体</th>
    <th>JSON 前缀</th>
    <th>是否可由地图创建？</th>
  </tr>
</thead>
<tbody>
  <tr><td>玩家</td><td><code>player</code></td><td>是</td></tr>
  <tr><td>墙</td><td><code>wall</code></td><td>是</td></tr>
  <tr><td>出口</td><td><code>exit</code></td><td>是</td></tr>
  <tr><td>巨石</td><td><code>boulder</code></td><td>是</td></tr>
  <tr><td>地面开关</td><td><code>switch</code></td><td>是</td></tr>
  <tr><td>门</td><td><code>door</code></td><td>是</td></tr>
  <tr><td>传送门</td><td><code>portal</code></td><td>是</td></tr>
  <tr><td>僵尸吐司生成器</td><td><code>zombie_toast_spawner</code></td><td>是</td></tr>
  <tr><td>蜘蛛</td><td><code>spider</code></td><td>是</td></tr>
  <tr><td>僵尸吐司</td><td><code>zombie_toast</code></td><td>是</td></tr>
  <tr><td>佣兵</td><td><code>mercenary</code></td><td>是</td></tr>
  <tr><td>宝藏</td><td><code>treasure</code></td><td>是</td></tr>
  <tr><td>钥匙</td><td><code>key</code></td><td>是</td></tr>
  <tr><td>无敌药水</td><td><code>invincibility_potion</code></td><td>是</td></tr>
  <tr><td>隐身药水</td><td><code>invisibility_potion</code></td><td>是</td></tr>
  <tr><td>木材</td><td><code>wood</code></td><td>是</td></tr>
  <tr><td>箭</td><td><code>arrow</code></td><td>是</td></tr>
  <tr><td>炸弹</td><td><code>bomb</code></td><td>是</td></tr>
  <tr><td>剑</td><td><code>sword</code></td><td>是</td></tr>
  <tr><td>弓</td><td><code>bow</code></td><td>否，该实体必须由玩家构建</td></tr>
  <tr><td>盾牌</td><td><code>shield</code></td><td>否，该实体必须由玩家构建</td></tr>
</tbody>
</table>

### 4.1.2 额外字段（MVP）

某些实体在其 JSON 条目中包含附加字段，具体如下：

* 所有 `portal` 类型的实体将包含一个 `colour` 字段。具有相同颜色的两个传送门是相连的（即穿过一个传送门会传送到另一个）。我们不会提供具有三个及以上相同颜色传送门的地下城，所有传送门都将成对出现。
* 所有 `door` 和 `key` 类型的实体将包含一个 `key` 字段。对于钥匙，该字段是钥匙的标识符；对于门，该字段是能打开该门的钥匙的标识符。

### 4.1.3 实体变体（MVP）

某些实体可能具有不同的变体，需向玩家展示。例如，传送门颜色和门的状态。处理这些变体的代码在 `dungeonmania/util/NameConverter.java` 中。

<table>
<thead>
  <tr>
    <th>基础实体</th>
    <th>变体</th>
    <th>说明</th>
  </tr>
</thead>
  <tr>
    <td>传送门</td>
    <td><code>portal_{COLOUR}</code>（例如 <code>portal_red</code>）</td>
    <td>传送门总是以颜色表示，该颜色将附加在 "portal" 前缀后。</td>
  </tr>
  <tr>
    <td>门</td>
    <td><code>door</code>，<code>door_open</code></td>
    <td>门开启时表示为 <code>door_open</code>，而关闭时仅为 <code>door</code>。</td>
  </tr>
</table>

如果你需要表示其他变体实体，可自行修改 `NameConverter.java`。

### 4.1.4 输入 - 目标（MVP）

一个基本目标在地下城中表示如下：

```
"goal-condition": {
    "goal": <goal>
}
```

其中 `<goal>` 为 `"boulders"`、`"treasure"` 或 `"exit"` 之一。

一个复合目标在地下城中表示如下：

```
"goal-condition": {
    "goal": <supergoal>,
    "subgoals": [
        {"goal": <goal>},
        {"goal": <goal>}
    ]
}
```

其中 `<goal>` 为 `"boulders"`、`"treasure"` 或 `"exit"` 之一，或是另一个嵌套的复合目标；`<supergoal>` 为 `"AND"` 或 `"OR"`。

## 4.2 配置文件

在 `config_template.json` 中，我们提供了配置文件的模板。该文件十分重要，它指定了内部的游戏机制，会影响你程序的外部行为。你必须在游戏创建时从该文件读取配置值，而不是将这些常量硬编码进你的类中。

在自动评分过程中，我们会为每个测试用地下城提供我们自己的配置文件——这允许我们的测试设置参数，以确保行为表现明确。因此，如果你未正确读取这些值，你很可能会在大量自动测试中失败。

除非特别说明，配置字段均为整数。
### 4.2.1 配置字段（MVP）

<table>
<thead>
  <tr>
    <th style="font-weight:bold">JSON 格式<br></th>
    <th style="font-weight:bold">说明</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td> <code>ally_attack</code>
  </td>
    <td>每个盟友给予玩家的攻击加成。</td>
  </tr>
  <tr>
    <td> <code>ally_defence</code>
  </td>
    <td>每个盟友给予玩家的敌人攻击伤害效果减弱。</td>
  </tr>
  <tr>
    <td> <code>bribe_radius</code>
    </td>
  <td>可以贿赂佣兵的半径范围。</td>
  </tr>
  <tr>
    <td> <code>bribe_amount</code>
  </td>
    <td>贿赂佣兵所需的金币数量。</td>
  </tr>
  <tr>
    <td> <code>bomb_radius</code>
  </td>
    <td>炸弹的爆炸半径。</td>
  </tr>
  <tr>
    <td> <code>bow_durability</code>
  </td>
    <td>弓可以使用的战斗次数。</td>
  </tr>
  <tr>
    <td> <code>player_health</code>
  </td>
    <td>角色的生命值。</td>
  </tr>
  <tr>
    <td> <code>player_attack</code>
  </td>
    <td>角色的攻击伤害。</td>
  </tr>
  <tr>
    <td> <code>invincibility_potion_duration</code>
  </td>
    <td>无敌药水的效果持续 x 个 tick。</td>
  </tr>
  <tr>
    <td> <code>invisibility_potion_duration</code>
  </td>
    <td>隐身药水的效果持续 x 个 tick。</td>
  </tr>
  <tr>
    <td> <code>mercenary_attack</code>
  </td>
    <td>佣兵的攻击伤害。</td>
  </tr>
  <tr>
    <td> <code>mercenary_health</code>
  </td>
    <td>佣兵的生命值。</td>
  </tr>
  <tr>
    <td> <code>spider_attack</code>
  </td>
    <td>蜘蛛的攻击伤害。</td>
  </tr>
  <tr>
    <td> <code>spider_health</code>
  </td>
    <td>蜘蛛的生命值。</td>
  </tr>
  <tr>
    <td> <code>spider_spawn_interval</code>
  </td>
    <td>蜘蛛每隔 x 个 tick 生成一次，从第 x 个 tick 开始。如果生成率为 0，则游戏中永远不会生成蜘蛛。</td>
  </tr>
  <tr>
    <td> <code>shield_durability</code>
  </td>
    <td>盾牌可以使用的战斗次数。</td>
  </tr>
  <tr>
    <td> <code>shield_defence</code>
  </td>
    <td>由于盾牌的存在而导致敌人攻击伤害效果的减弱。</td>
  </tr>
  <tr>
    <td> <code>sword_attack</code>
  </td>
    <td>玩家在战斗中使用剑时增加的攻击伤害数值。</td>
  </tr>
  <tr>
    <td> <code>sword_durability</code>
  </td>
    <td>剑可以使用的战斗次数。</td>
  </tr>
  <tr>
    <td> <code>treasure_goal</code>
  </td>
    <td>至少需要收集 x 个宝藏才能完成宝藏目标。</td>
  </tr>
  <tr>
    <td> <code>zombie_attack</code>
  </td>
    <td>僵尸吐司的攻击伤害。</td>
  </tr>
  <tr>
    <td> <code>zombie_health</code>
  </td>
    <td>僵尸吐司的生命值。</td>
  </tr>
  <tr>
    <td> <code>zombie_spawn_interval</code>
  </td>
    <td>每个生成器每隔 x 个 tick 生成一次僵尸，从第 x 个 tick 开始。如果生成率为 0，则游戏中永远不会生成僵尸。</td>
  </tr>
</tbody>
</table>

### 4.2.1 配置字段（MVP）

<table>
<thead>
  <tr>
    <th style="font-weight:bold">JSON 格式<br></th>
    <th style="font-weight:bold">说明</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td> <code>ally_attack</code>
  </td>
    <td>每个盟友给予玩家的攻击加成。</td>
  </tr>
  <tr>
    <td> <code>ally_defence</code>
  </td>
    <td>每个盟友给予玩家的敌人攻击伤害效果减弱。</td>
  </tr>
  <tr>
    <td> <code>bribe_radius</code>
    </td>
  <td>可以贿赂佣兵的半径范围。</td>
  </tr>
  <tr>
    <td> <code>bribe_amount</code>
  </td>
    <td>贿赂佣兵所需的金币数量。</td>
  </tr>
  <tr>
    <td> <code>bomb_radius</code>
  </td>
    <td>炸弹的爆炸半径。</td>
  </tr>
  <tr>
    <td> <code>bow_durability</code>
  </td>
    <td>弓可以使用的战斗次数。</td>
  </tr>
  <tr>
    <td> <code>player_health</code>
  </td>
    <td>角色的生命值。</td>
  </tr>
  <tr>
    <td> <code>player_attack</code>
  </td>
    <td>角色的攻击伤害。</td>
  </tr>
  <tr>
    <td> <code>invincibility_potion_duration</code>
  </td>
    <td>无敌药水的效果持续 x 个 tick。</td>
  </tr>
  <tr>
    <td> <code>invisibility_potion_duration</code>
  </td>
    <td>隐身药水的效果持续 x 个 tick。</td>
  </tr>
  <tr>
    <td> <code>mercenary_attack</code>
  </td>
    <td>佣兵的攻击伤害。</td>
  </tr>
  <tr>
    <td> <code>mercenary_health</code>
  </td>
    <td>佣兵的生命值。</td>
  </tr>
  <tr>
    <td> <code>spider_attack</code>
  </td>
    <td>蜘蛛的攻击伤害。</td>
  </tr>
  <tr>
    <td> <code>spider_health</code>
  </td>
    <td>蜘蛛的生命值。</td>
  </tr>
  <tr>
    <td> <code>spider_spawn_interval</code>
  </td>
    <td>蜘蛛每隔 x 个 tick 生成一次，从第 x 个 tick 开始。如果生成率为 0，则游戏中永远不会生成蜘蛛。</td>
  </tr>
  <tr>
    <td> <code>shield_durability</code>
  </td>
    <td>盾牌可以使用的战斗次数。</td>
  </tr>
  <tr>
    <td> <code>shield_defence</code>
  </td>
    <td>由于盾牌的存在而导致敌人攻击伤害效果的减弱。</td>
  </tr>
  <tr>
    <td> <code>sword_attack</code>
  </td>
    <td>玩家在战斗中使用剑时增加的攻击伤害数值。</td>
  </tr>
  <tr>
    <td> <code>sword_durability</code>
  </td>
    <td>剑可以使用的战斗次数。</td>
  </tr>
  <tr>
    <td> <code>treasure_goal</code>
  </td>
    <td>至少需要收集 x 个宝藏才能完成宝藏目标。</td>
  </tr>
  <tr>
    <td> <code>zombie_attack</code>
  </td>
    <td>僵尸吐司的攻击伤害。</td>
  </tr>
  <tr>
    <td> <code>zombie_health</code>
  </td>
    <td>僵尸吐司的生命值。</td>
  </tr>
  <tr>
    <td> <code>zombie_spawn_interval</code>
  </td>
    <td>每个生成器每隔 x 个 tick 生成一次僵尸，从第 x 个 tick 开始。如果生成率为 0，则游戏中永远不会生成僵尸。</td>
  </tr>
</tbody>
</table>

### 4.3 接口

抽象层位于控制器级别。在起始代码中，我们提供了一个类 `DungeonManiaController`。

控制器方法与我们为你编写的 Web 服务器形式的 HTTP 层进行交互。

#### 4.3.1 接口数据类型

我们在 `response/models` 中为你提供了以下接口数据类型。和作业中一样，你需要创建这些类型的对象，以便控制器返回并将信息传递给服务器层。

如果你感兴趣，服务器层会将这些对象包装在一个名为 `GenericResponseWrapper` 的泛型类型中，并使用一个名为 gson 的库将这些对象转换为 JSON，以便通过 HTTP 响应传输到前端。

<table>
<thead>
  <tr>
    <th style="font-weight:bold">构造函数原型<br></th>
    <th style="font-weight:bold">说明<br></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><code>public DungeonResponse(String dungeonId, String dungeonName, List&lt;EntityResponse&gt; entities, List&lt;ItemResponse&gt; inventory, List&lt;BattleResponse&gt; battles, List&lt;String&gt; buildables, String goals)</code></td>
    <td>
    <ul>
    <li><code>dungeonId</code> 是地牢的唯一标识符</li>
    <li><code>dungeonName</code> 是所使用地牢地图的名称（即 <code>maze</code>，对应文件 <code>src/main/resources/dungeons/maze.json</code>）</li>
    <li><code>entities</code> 是当前地牢中所有实体的列表（不包括玩家背包中的实体）；如果玩家或敌人死亡，则会从该列表中移除</li>
    <li><code>inventory</code> 是玩家当前的背包</li>
    <li><code>buildables</code> 是玩家在当前背包和游戏状态下可以建造的物品类型列表</li>
    <li><code>battles</code> 是到目前为止游戏中发生的所有战斗的列表（见 BattleResponse），按发生顺序排列</li>
    <li><code>goals</code> 是一个包含尚未完成目标的字符串。空字符串表示游戏已获胜。字符串中的每个目标前都有一个冒号 <code>:</code>，并且是第 2.7 节中列出的三个基本目标之一，或你将在任务 2a) 中实现的第四个目标。你如何表示合取（AND）和析取（OR）是你自己的选择，前端只会将你的字符串以图像的方式呈现。我们在测试中只会检查目标字符串（例如 <code>:exit</code>）。一个 <code>goals</code> 字符串的示例为 <code>":exit AND (:treasure OR :enemies)"</code></li>
    </ul>
    </td>
  </tr>
  <tr>
    <td><code>public BattleResponse(String enemy, List&lt;RoundResponse&gt; rounds, double initialPlayerHealth, double initialEnemyHealth, List&lt;ItemResponse&gt; weaponryUsed)</code></td>
    <td>
      <ul>
      <li> <code>enemy</code> 是敌人的类型（例如 spider）</li>
      <li><code>rounds</code> 表示战斗的各个回合（见 RoundResponse）</li>
      <li><code>initialPlayerHealth</code> 是玩家在战斗前的初始生命值</li>
      <li><code>initialEnemyHealth</code> 是敌人在战斗前的初始生命值</li>
      <li> <code>weaponryUsed</code> 是战斗中使用的所有攻击和防御物品的列表，包括药水</li>
      </ul>
    </td></tr>
  <tr>
    <td><code>public RoundResponse(double deltaPlayerHealth, double deltaEnemyHealth)</code></td>
    <td>
      <ul>
      <li> <code>deltaPlayerHealth</code> 是该战斗回合中角色生命值的变化（例如 -3 表示生命值减少 3）</li>
      <li><code>deltaEnemyHealth</code> 是该战斗回合中敌人生命值的相应变化</li>
      </ul>
      <br>注意，这些变化值可以为正数，并且生命值的“符号”很重要（例如，正数表示增加，负数表示减少）
    </td>
  </tr>
  <tr>
    <td><code>public EntityResponse(String id, String type, Position position, boolean isInteractable)</code></td>
    <td>
    <ul>
      <li><code>id</code> 是该实体的唯一标识符</li>
      <li><code>type</code> 是实体的类型（前缀对应第 4.1.1 节中的表格）</li>
      <li><code>position</code> 是实体的 x, y, z（层）坐标</li>
      <li><code>isInteractable</code> 表示该实体是否可以从前端接收交互更新，仅适用于佣兵和僵尸吐司生成器。当佣兵变成盟友时，它们将不再可交互</li>
    </ul>
    </td>
  </tr>
  <tr>
    <td><code>public ItemResponse(String id, String type)</code></td>
    <td>
    <ul>
      <li><code>id</code> 是该物品的唯一标识符</li>
      <li><code>type</code> 是物品的类型（小写，名称见第 4.1.1 节）</li>
    </ul>
    </td>
  </tr>
  <tr>
    <td><code>public Position(int x, int y, int layer)</code></td>
    <td>
    <ul>
      <li><code>x</code>, <code>y</code> 是该单元格的坐标（<b>左上角单元格为 0,0</b>）</li>
      <li><code>layer</code> 是实体在屏幕上的 Z 轴位置（图像上更高的层位于更低层的前面）。Z 轴位置只影响前端渲染，我们不会对其进行测试</li>
      </ul>
    </td>
  </tr>
    <tr>
    <td><code>public enum Direction { 
        UP(0, -1), 
        DOWN(0, 1), 
        LEFT(-1, 0),
        RIGHT(1, 0); 
      }</code></td>
    <td>玩家的移动方向。</td>
    </tr>
    </tbody>
</table>
### 4.2.1 配置字段（MVP）

<!-- 原内容略 -->

### 4.3.2 接口方法

<table>
<thead>
  <tr>
    <th style="font-weight:bold">方法原型<br></th>
    <th style="font-weight:bold">说明<br></th>
    <th style="font-weight:bold">异常<br></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><code>public DungeonResponse newGame(String dungeonName, String configName) throws IllegalArgumentException</code></td>
    <td>
     创建一个新游戏，其中 <code>dungeonName</code> 是地牢地图的名称（对应存储在 model 中的 JSON 文件），<code>configName</code> 是配置文件的名称。
    </td>
    <td>
      IllegalArgumentException：
      <ul>
        <li>如果 <code>dungeonName</code> 不是一个存在的地牢</li>
        <li>如果 <code>configName</code> 不是一个存在的配置</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td><code>public DungeonResponse getDungeonResponseModel()</code></td>
    <td>
      返回当前游戏状态下的地牢响应，不对游戏造成任何副作用。
    </td>
    <td>
      N/A
    </td>
  </tr>
  <tr>
    <td><code>public DungeonResponse tick(String itemUsedId) throws InvalidActionException</code></td>
    <td> 当玩家使用/尝试使用物品时推进游戏状态。玩家的行为（尝试/使用物品）必须先执行，<i>然后</i>敌人移动。一旦物品被使用，它会从背包中移除。</td>
    <td>
    IllegalArgumentException：
    <ul>
      <li>如果 <code>itemUsed</code> 不是 <code>bomb</code>、<code>invincibility_potion</code> 或 <code>invisibility_potion</code>。</li>
    </ul><br><br>
    InvalidActionException：
    <ul>
      <li>如果 <code>itemUsed</code> 不在玩家的背包中</li>
    </ul>
    </td>
  </tr>
  <tr>
    <td><code>public DungeonResponse tick(Direction movementDirection)</code></td>
    <td>
      当玩家向指定方向移动一个格子时推进游戏状态。玩家的移动必须先执行，<i>然后</i>敌人移动。
    </td>
    <td>
      N/A
    </td>
  </tr>
  <tr>
    <td><code>public DungeonResponse build(String buildable) throws InvalidActionException</code></td>
    <td>
    构建给定的实体，<code>buildable</code> 的取值为 <code>bow</code> 或 <code>shield</code>
    </td>
    <td>
    IllegalArgumentException：
    <ul><li>如果 <code>buildable</code> 不是 <code>bow</code> 或 <code>shield</code></li></ul><br><br>
    InvalidActionException：
    <ul><li>如果玩家没有足够的物品来合成该建造项</li></ul>
    </td>
  </tr>
  <tr>
    <td><code>public DungeonResponse interact(String entityId) throws IllegalArgumentException</code></td>
    <td>
    与佣兵或僵尸生成器进行交互：玩家贿赂佣兵，或摧毁生成器。
    </td>
    <td>
    IllegalArgumentException：
    <ul>
    <li>如果 <code>entityId</code> 不是一个有效的实体 ID</li></ul><br><br>
    InvalidActionException：
    <ul>
    <li>当玩家尝试贿赂佣兵时，其不在指定贿赂范围内</li>
    <li>如果玩家金币不足却试图贿赂佣兵</li>
    <li>当试图摧毁生成器时，玩家未与其在相邻的正交格子</li>
    <li>如果玩家没有武器却尝试摧毁生成器</li>
    </ul>
    </td>
  </tr>
</tbody>
</table>

### 4.3.3 接口异常

控制器可能抛出的两个异常为：

* `IllegalArgumentException`（内置的未检查异常），在指定条件下抛出；
* `InvalidActionException`（在 `src/main/java/dungeonmania/exceptions` 中定义的自定义已检查异常）。

你可以按任意顺序抛出这些异常，我们不会测试任何会同时触发多个异常的输入。

### 4.3.4 其他接口文件

<table>
<thead>
  <tr>
    <th style="font-weight:bold">文件<br></th>
    <th style="font-weight:bold">路径<br></th>
    <th style="font-weight:bold">描述<br></th>
    <th style="font-weight:bold">你需要修改它吗？<br></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><code>DungeonManiaController.java</code></td>
    <td><code>src/main/java/dungeonmania/DungeonManiaController.java</code></td>
    <td>包含你需要实现的每个命令的方法。</td>
    <td><b>是</b></td>
  </tr>
  <tr>
    <td><code>App.java</code></td>
    <td><code>src/main/java/App.java</code></td>
    <td>为 Dungeon Mania 运行服务器。</td>
    <td><b>否</b></td>
  </tr>
  <tr>
    <td><code>Position.java, Direction.java,</code> 和 <code>FileLoader.java</code></td>
    <td><code>src/main/java/dungeonmania/util/Position.java, src/main/java/dungeonmania/util/FileLoader.java,</code> 和 <code>src/main/java/dungeonmania/util/Direction.java</code></td>
    <td>见第 4.3.1 节</td>
    <td><b>否 - 请不要修改这些文件，我们会在自动评分时依赖它们保持不变。</b></td>
  </tr>
  <tr>
    <td><code>DungeonResponse.java, EntityResponse.java, GenericResponseWrapper.java,</code> 和 <code>ItemResponse.java</code></td>
    <td><code>src/main/java/dungeonmania/response/models/</code></td>
    <td>见第 4.3.1 节</td>
    <td><b>否。</b></td>
  </tr>
  <tr>
    <td><code>Scintilla.java</code> 及辅助文件；<code>Environment.java, PlatformUtils.java,</code> 和 <code>WebServer.java</code></td>
    <td><code>src/main/java/scintilla</code></td>
    <td>包含围绕 Spark-Java 构建的小型自定义封装器，用于运行 Web 服务器。运行时会自动打开网页浏览器。</td>
    <td><b>否。</b></td>
  </tr>
  <tr>
    <td><code>InvalidActionException.java</code></td>
    <td><code>src/main/java/dungeonmania/exceptions</code></td>
    <td>当尝试执行无效操作时抛出的受检异常（见第 4.3.5 节）。</td>
    <td><b>否 - 请不要修改此类，我们会在自动评分中依赖它。</b></td>
  </tr>
</tbody>
</table>

### 4.3.4 游戏玩法

虽然你只负责改进后端部分，但我们为你提供了一个前端示例。

游戏要求你选择一个地牢文件和一个配置文件。地牢文件和配置文件可以通过将它们分别放入 `main/resources/dungeons` 和 `main/resources/configs` 文件夹中来加载到前端。

玩家角色可以使用 WASD 键移动。建造和使用物品可以通过鼠标点击背包中的相应图标完成。交互可以通过点击游戏地图上的相关实体来执行。

由于前端不是产品需求，因此所提供的前端仅为示例，可能包含错误。
