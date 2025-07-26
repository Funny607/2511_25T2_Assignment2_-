# Assignment-ii <!-- omit in toc -->

## 截止日期：第10周星期五，下午3点 [悉尼本地时间](https://www.timeanddate.com/worldclock/australia/sydney)（2025年8月8日）

- [1. 第1部分 \[60分\]](#1-part-1-60-marks)
- [2. 第2部分 \[40分\]](#2-part-2-40-marks)
- [3. 其他信息](#3-other-information)
  - [3.1. 迟交处罚](#31-late-penalties)
  - [3.2. 特殊情况](#32-extenuating-circumstances)
  - [3.3. 其他要求](#33-other-expectations)
  - [3.4. 抄袭](#34-plagiarism)
  - [3.5. 版权声明](#35-copyright-notice)
- [4. 致谢](#4-credits)

重点说明：

- 本次作业包含**两个**部分。
- **截止时间之后不接受任何提交，包括有特殊情况或平等学习计划（ELP）的学生。** 详见[此处](#3-other-information)。
- 我们将以你 `main` 分支的内容作为第1部分的最终提交版本。详见[此处](#3-other-information)。
- 本作业不接受特殊情况所提供的“短期延期”。详见[此处](#32-extenuating-circumstances)。
- 请确保你的第1部分提交通过我们的 dryrun 检查：[点此查看](./part-1/Part1_Specification.md#63-dryruns)。
- 有关特殊情况或平等学习计划，请访问[课程网站](https://cgi.cse.unsw.edu.au/~cs2511/25T2/els-spec-cons)。
- 第2部分必须提交到 [Moodle](https://moodle.telt.unsw.edu.au/mod/turnitintooltwo/view.php?id=7987670)。

# 1. 第1部分 [60分]

本次作业将提供一个已经开发好的系统。你需要首先分析并重构代码，然后根据需求变化对系统进行适配。

本部分的目标可分为五个核心主题：

1. **熟悉现有系统。** 对你们中的许多人来说，这可能是迄今为止接触过的最大代码库——可能会令人望而生畏！能够处理自己没从头开发、也不完全理解的系统是软件工程师必备的技能。
2. **重构技术。** 就像你露营时要把营地打扫干净再离开一样，你也应该在离开代码前把它变得更好。我们特意在系统中放置了一些设计缺陷和“设计异味”，供你发现和重构。
3. **设计模式。** 能够在已有代码中识别模式，并使用设计模式提高代码质量是软件工程师的重要技能。你将有机会应用课程中讲授的理论。
4. **需求演进。** 软件从不是静止的——它总是在发展和变化中。你将基于现有系统来适配这些变化，进行迭代式的设计和开发。
5. **应对未知。** 本作业中存在许多未知问题，你和你的伙伴需要一同探索、调查、澄清并在必要时寻求帮助，以从容应对挑战并取得成功。

本说明分为**两个**部分：

1. [产品规范 - MVP](./part-1/MVP.md)：描述现有 MVP 实现的原始需求。
2. [第1部分规范](./part-1/Part1_Specification.md)：描述你需要完成的任务。

我们还附上了一些额外文档，供你参考：

- [系统设置指南](./part-1/Setup.md)
- [个性化设置](./part-1/Customisations.md)
- [博客说明](./part-1/Blog_Instructions.md)
- [地下城地图创建工具](https://cs2511-dungeonmania-map-generator.vercel.app)（仅供参考，可能不完整）

# 2. 第2部分 [40分]

> 本部分将于第8周星期一发布。

第2部分重点是第7周及以后介绍的内容。两个教程和实验课将深入探讨与本部分相关的关键主题。在此部分中，你将建模一个系统的高级架构，聚焦其结构性和行为性方面。案例研究将基于现实、贴近行业的需求，以提供实际的设计体验。这些模型将使用 C4 架构表示法和行为建模技术进行开发。

- [第2部分规范](./part-2/Part2_Specification.md)

# 3. 其他信息

## 3.1. 迟交处罚

**截止日期之后不接受任何提交，包括有特殊情况和 ELP 的学生。**

## 3.2. 特殊情况

关于成绩宽限的批准必须通过特殊情况申请或预先安排的平等学习计划完成。详细信息请访问[课程网站](https://cgi.cse.unsw.edu.au/~cs2511/25T2/els-spec-cons#23-assignment-ii)。如有疑问，请发送邮件至 [cs2511@cse.unsw.edu.au](mailto:cs2511@cse.unsw.edu.au)。

## 3.3. 其他要求

虽然你们小组可以自行决定工作分配方式，但在评估时每位成员都必须达到以下关键标准：

- 代码贡献；
- 非代码贡献；
- 使用 Git/GitLab；
- 撰写个人博客；
- 遵守学术规范。

> 通常两位成员将获得相同的分数，但如果某成员未达到上述标准，其作业总评分将被降低。
>
> 如果你认为你的组员没有尽到应有的责任，你必须在对应周的结束前通知你的导师。
>
> 例如，如果你组员在第7周没有贡献内容，你需要在第7周结束前报告此事。不要延迟。如果未及时报告，我们可能无法介入或重新分配分数。

## 3.4. 抄袭

你们的程序必须完全由你的小组独立完成。我们将使用抄袭检测软件对所有提交进行两两比较（包括往年类似作业），并对抄袭行为严惩不贷，包括记录在 UNSW 的抄袭档案中。如果有奖学金持有者涉及抄袭或其他不当行为，将通报相关奖学金机构。

你们**不得**提交使用 ChatGPT、GitHub Copilot、Gemini 或类似自动工具生成的代码。

- 不得从你的小组以外的其他人处复制想法或代码。
- 不得使用公共代码仓库，也不得让非小组成员查看你们的代码，教学团队除外。
- 使用 ChatGPT、Copilot、Gemini 等工具生成的代码将被视为抄袭。

课程组保留要求你向教学人员解释你的代码或设计选择的权利，或要求你完成类似评估。

请参考以下在线资源了解 UNSW 关于抄袭的规定及处理方式：

- [学术诚信与抄袭](https://www.student.unsw.edu.au/plagiarism/integrity)
- [UNSW 抄袭政策](https://www.unsw.edu.au/content/dam/pdfs/governance/policy/2022-01-policies/plagiarismpolicy.pdf)

## 3.5. 版权声明

擅自复制、发布、分发或翻译本作业将被视为侵犯版权行为，并将上报 UNSW 学生行为与诚信办公室处理。

# 4. 致谢

如未明确声明许可协议，则视为公共领域资源（可用于商业或非商业用途），但未提供明确授权。

- 前端与整体系统由 cs2511 开发：Braedon Wooding、Nick Patrikeos、George Litsas、Noa Challis、Chloe Cheong、Sienna Archer、Tina Ji、Webster Zhang、Adi Kishore、Amanda Lu
- 魔戒：由 Jordan Irwin (AntumDeluge) 创建
- 雇佣兵：由 Calciumtrice 制作的“动画游侠”，采用 CC BY 3.0 授权协议
- 传送门：由 RodHakGames - RHG 制作
- 巨石：由 Viktor Hahn (Viktor.Hahn@web.de) 制作，采用 CC BY 4.0 授权协议
- Alagard 字体：由 Pix3M 制作，采用 CC BY 3.0 授权协议
- 护甲与盾牌：由 Zeno 制作
- 图块与随机实体：由 egordorichev 制作，属于公共领域（CC0）
- 金币/宝藏：由 La Red Games 提供
- 僵尸吐司：由 LHTeam (LazyHamsters) 制作
- 烤面包机：由 Reakain 制作；许可：可用于免费或商业项目，可自行修改，无需署名（但署名将被感谢），禁止重新分发或转售
- 蜘蛛：由 Elthen 制作
- 本作业由 Robert Clifton-Everest、Braedon Wooding、Ashesh Mahidadia、Fethi Rabhi、Nick Patrikeos 和 Amanda Lu 编写
