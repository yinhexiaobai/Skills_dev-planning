# dev-planning — Claude Code Skill

> **先规划，再编码。** 这是一个 Claude Code Skill，强制在编码开始前完成开发规划。

---

## 简介

`dev-planning` 是一个专为 Claude Code 设计的 Skill，用于在任何编码任务开始前，驱动 Claude 完成结构化的开发规划。它通过四步流程，帮助开发者厘清需求、制定技术方案、拆分任务，从而降低返工风险、提升开发效率。

---

## 触发场景

当你要求 Claude 完成以下任务时，该 Skill 会自动介入：

- 实现新功能 / 开发模块
- 创建新项目
- 重构现有代码
- 修复非单行的 Bug

---

## 工作流程

```
提问澄清（≥3个问题）
      ↓
检测项目类型 → 确定规划文件路径
      ↓
生成并写入开发规划文件
      ↓
逐任务推进编码
```

### 第一步：主动提问

从需求、技术、约束三个维度，一次性向用户提出 ≥ 3 个问题，**不开始写任何代码**，等待用户回答。

### 第二步：检测项目类型

根据项目根目录中的特征文件自动判断项目类型，决定规划文件的存放路径：

| 项目类型 | 特征文件 | 规划文件路径 |
|---------|---------|------------|
| Java / Maven | `pom.xml` | `docs/dev-plan.md` |
| Java / Gradle | `build.gradle` | `docs/dev-plan.md` |
| Python | `pyproject.toml` / `requirements.txt` | `docs/dev-plan.md` |
| Node.js 前端 | `package.json` + `src/` | `DEV_PLAN.md` |
| Go | `go.mod` | `docs/dev-plan.md` |
| Rust | `Cargo.toml` | `docs/dev-plan.md` |
| 纯前端 | `index.html` | `DEV_PLAN.md` |
| 通用 | 无匹配 | `.claude/dev-plan.md` |

### 第三步：生成开发规划文件

规划文件包含以下四个章节，所有章节均为必填：

1. **需求分析** — 背景目标、功能范围、验收标准
2. **技术方案** — 技术选型、模块设计、关键接口/数据结构
3. **任务拆分** — 每个子任务 1–4 小时内可完成
4. **风险与注意事项** — 风险点及应对措施

### 第四步：开始编码

规划完成后告知用户规划文件路径、任务摘要，并按任务列表逐项推进，完成一项打勾一项。

---

## 安装

将本仓库克隆或复制到 Claude Code 的 Skills 目录下：

```bash
# 克隆到全局 Skills 目录
git clone https://github.com/yinhexiaobai/Skills_dev-planning.git \
  ~/.claude/skills/dev-planning
```

重启 Claude Code 后 Skill 即可生效。

---

## 文件结构

```
dev-planning/
├── SKILL.md      # Skill 定义文件（核心，请勿修改）
└── README.md     # 说明文档（本文件）
```

---

## 版本

| 版本 | 说明 |
|------|------|
| v1.0 | 初始版本，四步规划流程 |

---

## License

MIT
