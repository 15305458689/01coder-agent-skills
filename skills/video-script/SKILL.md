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

```markdown
# {视频标题}

## 开场 (Hook)
<!-- 前15-30秒，抓住观众注意力 -->
<!-- 提出问题/展示结果/制造好奇心 -->

## 引言
<!-- 简要介绍今天要讲的内容 -->
<!-- 说明为什么这个主题重要/有价值 -->

## 正文

### 要点一：{小标题}
<!-- 讲解内容 -->

### 要点二：{小标题}
<!-- 讲解内容 -->

### 要点三：{小标题}
<!-- 讲解内容 -->

<!-- 根据需要增减要点 -->

## 演示 (Demo) — 如有
<!-- 屏幕操作步骤说明 -->

## 总结
<!-- 回顾要点 -->
<!-- 给出行动建议 -->

## 结尾 (CTA)
<!-- 引导点赞、关注、评论 -->
```

#### Script Writing Guidelines

1. **口语化**：写出来的是要说的话，不是文章。用短句，避免书面语。
2. **节奏感**：每个段落不要太长，适合口播节奏。关键概念后留停顿。
3. **具体化**：用具体例子和类比解释抽象概念。
4. **过渡自然**：段落之间用口语化过渡（"接下来"、"说到这个"、"那么问题来了"）。
5. **时间把控**：中文口播约 200-250 字/分钟，英文约 130-150 词/分钟。根据目标时长控制篇幅。

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

**User**: 帮我写一期视频脚本，主题是 Docker 入门，面向编程初学者，大约8分钟

**Claude**: Confirms understanding, creates `./2026-03-07-docker-intro/`, generates script.md and meta.md.

**meta.md (excerpt)**:

```markdown
# 视频发布素材

## 标题

5分钟学会 Docker：从零开始的容器化之旅

## 标题备选

- Docker 新手教程：告别"在我电脑上能跑"
- 编程初学者必看：Docker 到底是什么？

## 描述

还在为环境配置头疼吗？Docker 帮你一键搞定。这期视频从零开始，用最通俗的方式带你理解 Docker 的核心概念。

#docker #容器化 #编程入门 #devops #教程

- 加入社群：https://example.com/community
- Twitter: https://twitter.com/example
- Bilibili: https://space.bilibili.com/example
- 邮箱：contact@example.com

本期视频涵盖 Docker 的基本概念、安装、常用命令和第一个容器实战。

## 标签 (Tags)

#docker #容器 #编程入门 #devops #教程 #docker入门 #容器化

## 章节 (Chapters)

00:00 引言
00:25 Docker 是什么？一个类比讲清楚
01:30 为什么你需要 Docker
02:45 安装 Docker
03:30 核心概念：镜像 vs 容器
04:45 动手：运行第一个容器
06:30 常用命令速查
07:30 总结与下一步

## 封面建议

Docker 鲸鱼 logo 居中，背景用浅蓝色渐变，左上角放"入门教程"标签，整体风格简洁现代。
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
