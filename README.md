# 周报审校与整合 AI 技能（weekly-report-review）

> 一个把"老师傅的行政工作经验"固化下来的 AI Agent Skill
> 基于 [Agent Skills 标准](https://agentskills.io) · 兼容 Claude Code / Codex / Pi / ZCode

## 背景：这个技能解决什么问题

综合管理岗每周一要做一件高重复、高要求的工作——**把五个条线（综合管理、平台运营、国际市场、政策研究、国内市场）的周报，加上领导提供的流水账式周报，整合成一份结构规范、用语准确的上报材料**。

难点在于：
- 条线来源 ≠ 归类板块，要以**内容主题**为准归类；
- 会议、接待类事项有统一的归口规范；
- 行政用语、公司术语、易混词有固定用法，出错会被打回。

这些经验此前只存在于"做过的人"脑子里。我把它们**显性化为一份可执行、可维护的技能包**。

## 技能能力

| 能力 | 说明 |
|------|------|
| **五大板块归类** | 按预定义 taxonomy（`references/report-taxonomy.md`）逐条归类并给出理由 |
| **结构检查** | 对照标准结构检查缺失项、冗余项、流水账问题 |
| **规范检查** | 用词、格式、行政规范逐条过检（`references/error-checklist.md`） |
| **易混词纠错** | 常见误用词对照表（`references/word-usage.md`），如"报送/报备"等 |
| **修改意见输出** | 输出工作底稿（分类建议+问题清单+修改意见），**最终定稿由人完成** |

## 技能结构

```
weekly-report-review/
├── SKILL.md                 # 核心指令：触发条件 + 工作流程（frontmatter 描述驱动自动加载）
└── references/              # 领域知识沉淀
    ├── report-taxonomy.md   # 五大板块分类标准
    ├── error-checklist.md   # 逐条检查清单
    ├── word-usage.md        # 易混词用法
    └── writing-standards.md # 写作与格式规范
```

## 设计亮点

1. **渐进式披露**：Agent 启动时只加载技能的"名字+描述"，接到相关任务才读完整指令——不浪费上下文
2. **知识即文件**：术语、标准、易错点全部外置为可维护的 reference，随工作迭代持续更新
3. **人机分工边界清晰**：AI 做分类、检查、建议（可批量、可追溯），人做决策与定稿
4. **工具无关**：遵循开放标准，输入一段提示词或一个文件路径即可在任何主流 Agent 上运行

## 使用示例

```
用户：这是我整理这周的五个条线周报和领导流水账，帮我看看归类对不对、有没有规范问题。
Agent：按五大板块逐条归类 → 输出分类建议表 → 逐条检查用词格式 → 给出修改意见（不改写定稿）
```

## 快速开始

```bash
# 任选一个 agent，把本目录放入技能目录即可
cp -r weekly-report-review ~/.pi/agent/skills/    # Pi 示例
cp -r weekly-report-review ~/.codex/skills/       # Codex 示例
```

## 技术要点

- 标准：[Agent Skills](https://agentskills.io)（Anthropic 提出，已被主流 agent 采用）
- 触发：frontmatter `description` 语义匹配自动加载，或手动 `/skill:weekly-report-review`
- 知识库联动：技能内引用公司知识库路径，实现动态读取与校验