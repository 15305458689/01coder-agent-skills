---
name: video-script
description: Create video scripts and publishing materials for YouTubers/UP主. Use when user wants to prepare a video, write a script (口播稿), generate video title, description, tags, or chapter timestamps. Triggers on "写视频脚本", "视频口播稿", "video script", "prepare video", "视频发布素材", or mentions creating content for YouTube/Bilibili.
---

# Video Script & Publishing Materials

Help YouTubers/UP主 prepare video content: write structured scripts (口播稿) and generate publishing materials (title, description, tags, chapters).

## Output Structure

Each video gets a date-based directory under user's chosen location:

```
./videos/{YYYY-MM-DD}-{short-slug}/
├── script.md       # 视频口播稿
└── meta.md         # 发布素材 (title, description, tags, chapters)
```

## Interactive Workflow

### Step 1: Gather Information

**IMPORTANT**: Before writing anything, collect sufficient context from the user. Ask the user:

```
请提供以下信息，帮我为你准备视频内容：

1. **视频主题**：这期视频讲什么？（必填）
2. **目标平台**：YouTube / Bilibili / 两者都有？（默认：两者）
3. **目标时长**：大约几分钟？（默认：10分钟）
4. **目标观众**：面向什么人群？（如：开发者、AI爱好者、初学者）
5. **关键要点**：你希望视频覆盖哪些要点？（可以是大纲、笔记、或链接）
6. **相关资料**：有没有参考文章、文档、代码仓库？（可选，我可以帮你研究）
已有部分信息的话，直接告诉我就好，缺的我会追问。
```

If the user provides partial info upfront, only ask for the missing pieces.

### Step 2: Research (If Needed)

If the user provides reference URLs, docs, or repos:
- Use WebFetch to read reference articles/docs
- Use Read/Grep/Glob to explore code repos
- Use WebSearch to find supplementary information
- Summarize key points for script use

If the topic is about a specific technology/tool:
- Research its core features and selling points
- Find common pain points it solves
- Look for comparison angles with alternatives

### Step 3: Create Directory

Create the date-based directory:

```
./videos/{YYYY-MM-DD}-{short-slug}/
```

Example: `./videos/2026-03-07-react-server-components/`

The `short-slug` should be a brief, descriptive kebab-case label derived from the topic.

### Step 4: Write Script (script.md)

Write a structured 口播稿 in the user's preferred language. The script should be conversational and natural for speaking.

#### Script Structure

脚本应该像自然对话一样流畅，不要用"开场/引言/正文/总结/CTA"这种模板化结构。按内容逻辑分段，每段用描述性标题 + 时间估算。

```markdown
---
title: "{视频标题}"
date: "{YYYY-MM-DD}"
duration: "~X 分钟"
platform: "YouTube / Bilibili"
---

# {视频标题}

## Hook（0:00 - 0:10）

{一句话抓住注意力 — 展示结果、抛出问题、或制造好奇心}

大家好我是{名字}，{一两句话引出今天的主题和为什么值得看}。

## {第一部分描述性标题}（0:10 - 1:30）

{内容}

## {第二部分描述性标题}（1:30 - 3:00）

{内容}

（展示 xxx 对比示例）

## {第三部分描述性标题}（3:00 - 5:00）

我们现在直接来操作。

（录屏开始）

{操作步骤说明}

<!-- TODO: 补充实际操作截图 -->

## {收尾}（5:00 - 6:00）

{自然收束，回顾要点，给出行动建议}

好了今天的分享就到这里，感谢大家收看，我们下期再见。
```

#### Script Writing Guidelines

1. **Hook 极短**：第一句话就要抓人 — 用一个结果、一个反直觉的事实、或一个问题。然后自然带出自我介绍和主题背景。
2. **按内容分段，不按模板**：章节标题是描述性的（"安装配置"、"登录演示"），不是编号式的（"要点一"、"第三章"）。每段标题后标注时间估算 `(起始 - 结束)`。
3. **口语化**：写出来的是要说的话，不是文章。用短句，用"先澄清一点"、"那为什么还要折腾"、"问题出在...上"这种口语表达。
4. **制作备注**：在需要录屏、展示素材的地方加制作提示，用 `（录屏开始）`、`（展示 xxx）` 格式。未确定的内容用 `<!-- TODO: 补充... -->` 标记。
5. **结尾自然**：不需要仪式感的"请点赞关注"。简短回顾，自然收束。
6. **时间把控**：中文口播约 200-250 字/分钟，英文约 130-150 词/分钟。根据目标时长控制篇幅。

### Step 5: Write Meta (meta.md)

Generate publishing materials:

```markdown
# 视频发布素材

## 标题

{主标题}

## 标题备选

- {备选标题1}
- {备选标题2}

## 描述

{视频描述，概括视频内容，2-3句话}

{推广信息区域 — 由用户配置，见下方"个人推广信息"章节}

{视频内容摘要或补充说明}

## 标签 (Tags)

#tag1 #tag2 #tag3 #tag4 #tag5

## 章节 (Chapters)

00:00 引言
00:30 {章节1}
03:15 {章节2}
06:00 {章节3}
08:30 总结

## 封面建议

{简要描述适合这期视频的封面风格和元素}
```

#### Title Writing Guidelines

1. **简洁有力**：控制在 50 字符内（YouTube 推荐）
2. **包含关键词**：便于搜索发现
3. **制造好奇/价值感**：让人想点进来
4. **避免标题党**：真实反映内容
5. **提供备选**：至少给 2-3 个标题方案

#### Description Guidelines

1. 前两行最重要（折叠前可见），放核心信息
2. 包含相关关键词，利于 SEO
3. 包含用户的个人推广链接（见下方配置）

#### Chapter Timestamps

1. 必须从 `00:00` 开始（YouTube 要求）
2. 根据脚本结构估算时间点
3. 章节名简洁，不超过 30 字符
4. 提醒用户：时间戳需在视频剪辑完成后根据实际时长调整

### Step 6: Review & Iterate

完成后提示用户：

```
视频脚本和发布素材已准备好：

📂 {目录路径}/
├── script.md    — 口播稿（约 X 分钟）
└── meta.md      — 发布素材

请检查内容，如果需要调整，告诉我：
- 需要修改哪个部分？
- 风格/语气需要调整吗？
- 有要补充的要点吗？

提示：meta.md 中的章节时间戳是估算值，请在剪辑完成后根据实际时长调整。
```

## Personal Promotion Info

Users should configure their promotion block. On first use, ask the user for their promotion links and save to **auto memory** for cross-session persistence.

When asking:

```
我注意到这是你第一次使用视频脚本 skill。请提供你的个人推广信息，我会记住以便后续使用：

- 社交媒体链接（Twitter、Bilibili、YouTube 等）
- 知识星球/社群链接
- 联系方式
- 其他固定推广信息（如课程链接、赞助信息等）
```

Save to auto memory directory as `video-promo.md` (e.g. `~/.claude/projects/.../memory/video-promo.md`). On subsequent uses, check if this file exists in the memory directory and read it directly — no need to ask again.

## Examples

### Example 1: User provides topic directly

**User**: 帮我写一期视频脚本，主题是用 Turborepo 管理 monorepo，面向前端开发者，大约6分钟

**Claude**: Confirms understanding, creates `./videos/2026-03-07-turborepo-monorepo/`, generates script.md and meta.md.

**script.md (excerpt)**:

```markdown
---
title: "Turborepo：让你的 monorepo 构建快 10 倍"
date: "2026-03-07"
duration: "~6 分钟"
platform: "YouTube / Bilibili"
---

# Turborepo：让你的 monorepo 构建快 10 倍

## Hook（0:00 - 0:10）

我的项目有 12 个包，完整构建从 8 分钟降到了 45 秒。

大家好我是 xxx，今天来聊聊 Turborepo — 它是怎么做到这件事的，以及你的项目该不该用它。

## 为什么 monorepo 构建这么慢（0:10 - 1:00）

先说问题。你有一个 monorepo，里面放了前端应用、组件库、工具函数、后端服务。改了一行代码，CI 把所有包全构建一遍。

其实大部分包根本没变。问题出在构建工具不知道包之间的依赖关系，只能全量跑。

## Turborepo 的核心思路（1:00 - 2:30）

Turborepo 做的事情很简单 — 它分析包之间的依赖图，只构建真正受影响的包。

再加上远程缓存，同一份代码在 CI 上构建过一次，你本地 pull 下来就不用再跑了。

（展示 turbo run build 的输出对比：全量 vs 增量）

## 上手配置（2:30 - 4:30）

我们直接来操作。

（录屏开始）

<!-- TODO: 补充实际安装和 turbo.json 配置步骤 -->

首先在项目根目录安装 Turborepo……

然后是 turbo.json 的配置……

## 实际效果（4:30 - 5:30）

（展示构建时间对比截图）

<!-- TODO: 补充实际数据 -->

## 收尾（5:30 - 6:00）

Turborepo 不是银弹，小项目没必要上。但如果你的 monorepo 已经开始因为构建速度拖慢开发效率，它值得试一下。

好了今天的分享就到这里，感谢大家收看，我们下期再见。
```

**meta.md (excerpt)**:

```markdown
# 视频发布素材

## 标题

Turborepo：让你的 monorepo 构建快 10 倍

## 标题备选

- monorepo 构建太慢？Turborepo 帮你解决
- 告别全量构建：Turborepo 实战

## 描述

monorepo 项目构建越来越慢？Turborepo 通过依赖图分析和远程缓存，只构建真正变化的包。这期视频从原理到实操，带你快速上手。

#turborepo #monorepo #前端工程化 #vercel

{推广信息}

## 标签 (Tags)

#turborepo #monorepo #前端 #构建优化 #vercel #工程化

## 章节 (Chapters)

00:00 引言
00:10 为什么 monorepo 构建慢
01:00 Turborepo 核心思路
02:30 上手配置
04:30 实际效果
05:30 总结

## 封面建议

Turborepo logo + 构建速度对比（8min → 45s），背景用深色调，突出速度提升数字。
```

### Example 2: User provides reference material

**User**: 我想做一期关于 Cursor vs Windsurf 的对比视频，这是我的笔记 ./notes/cursor-vs-windsurf.md

**Claude**: Reads the notes file, researches both tools if needed, asks about target audience and length, then generates script and meta.

### Example 3: User only provides a vague idea

**User**: 想做一期视频聊聊 AI 编程

**Claude**:

```
好的，AI 编程是个很好的话题。让我了解更多细节：

1. 你想聊哪个方面？比如：
   - AI 编程工具对比（Cursor、Copilot、Claude Code 等）
   - AI 编程的工作流和最佳实践
   - AI 对程序员的影响和未来趋势
   - 某个具体工具的深度体验
2. 面向什么观众？开发者还是泛科技人群？
3. 大概多长时间？
```

## Critical Rules

1. **先问后写** — 信息不足时必须追问，不要猜测用户意图
2. **口语化脚本** — 脚本是用来说的，不是用来读的
3. **时间戳是估算** — 明确提醒用户剪辑后需调整
4. **不要自动发布** — 只生成文件，不执行任何发布操作
5. **保留用户风格** — 如果用户提供了之前的视频风格参考，尽量保持一致
6. **推广信息复用** — 首次询问后保存到 auto memory，后续自动填充
7. **日期目录** — 每期视频按当天日期创建独立目录
