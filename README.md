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
- 性能优化
- 架构调整

支持中英文任务描述。

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

从需求、技术、约束、AI/ML（如适用）四个维度，一次性向用户提出 3-5 个问题，**不开始写任何代码**，等待用户回答。

### 第二步：检测项目类型

根据项目根目录中的特征文件自动判断项目类型，决定规划文件的存放路径。

**支持 20+ 种项目类型：**

| 项目类型 | 特征文件 | 规划文件路径 |
|---------|---------|------------|
| Java / Maven | `pom.xml` | `docs/dev-plan.md` |
| Java / Gradle / Android | `build.gradle` | `docs/dev-plan.md` |
| Spring Boot | `src/main/java` | `docs/dev-plan.md` |
| Dart / Flutter | `pubspec.yaml` | `docs/dev-plan.md` |
| iOS / Swift / Xcode | `*.xcodeproj` | `docs/dev-plan.md` |
| Ruby / Rails | `Gemfile` | `docs/dev-plan.md` |
| PHP / Laravel | `composer.json` | `docs/dev-plan.md` |
| .NET / C# | `*.csproj` / `*.sln` | `docs/dev-plan.md` |
| Python / AI·ML | `pyproject.toml` + ML 依赖 | `docs/dev-plan.md` |
| Python | `requirements.txt` | `docs/dev-plan.md` |
| Go | `go.mod` | `docs/dev-plan.md` |
| Rust | `Cargo.toml` | `docs/dev-plan.md` |
| TypeScript | `tsconfig.json` | `DEV_PLAN.md` |
| React Native | `package.json` + `react-native` | `DEV_PLAN.md` |
| Vue / Nuxt | `package.json` + `vue`/`nuxt` | `DEV_PLAN.md` |
| Next.js | `package.json` + `next` | `DEV_PLAN.md` |
| Node.js / 前端 | `package.json` + `src/` | `DEV_PLAN.md` |
| 纯前端 | `index.html` | `DEV_PLAN.md` |
| Monorepo | 多语言特征文件共存 | `.claude/dev-plan.md` |
| 通用 | 无匹配 | `.claude/dev-plan.md` |

### 第三步：生成开发规划文件

规划文件包含以下五个章节，所有章节均为必填：

1. **需求分析** — 背景目标、功能范围、验收标准
2. **技术方案** — 架构概览、技术选型、模块设计、关键接口/数据结构、依赖清单
3. **任务拆分** — 每个子任务 1–4 小时内可完成
4. **环境与部署** — 本地/测试/生产环境差异、部署步骤
5. **风险与注意事项** — 风险点及应对措施

### 第四步：开始编码

规划完成后告知用户规划文件路径、任务摘要，并按任务列表逐项推进，完成一项打勾一项。

---

## 特色功能

### AI/ML 项目专项支持

检测到 AI/ML 项目时，自动增加专项提问维度：
- 模型或算法选择
- 数据集来源与预处理
- 模型效果评估指标
- 实验追踪工具

### Monorepo 支持

自动识别多语言 Monorepo，规划中明确列出受影响的子项目和改动顺序。

### 多语言支持

- 中文任务 → 中文提问 + 中文规划
- 英文任务 → 英文提问 + 英文规划

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
├── SKILL.md           # Skill 定义文件（核心）
├── README.md          # 说明文档（本文件）
└── evals/
    └── evals.json     # 评估测试集（5 个测试用例）
```

---

## 版本历史

| 版本 | 日期 | 说明 |
|------|------|------|
| v1.1 | 2026-02-28 | 扩展项目类型检测（20+ 种）、优化规划模板（五章节）、新增完整示例、优化触发描述 |
| v1.0 | 2026-02-28 | 初始版本，四步规划流程 |

---

## License

MIT
