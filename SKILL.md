---
name: dev-planning
description: >
  在开始任何编码任务前，先进行开发规划。当用户要求实现功能、开发模块、
  创建项目、重构代码、修复 Bug（非单行修复）时触发。
  强制执行：提问澄清 → 写入开发规划文件 → 再开始编码。
  绝对不允许跳过规划直接写代码。

  触发场景包括但不限于：新增功能（add feature, implement, build, create）、
  代码重构（refactor, restructure, reorganize）、Bug 修复（fix bug, resolve issue）、
  性能优化（optimize, improve performance）、架构调整（architecture change）、
  新建项目（new project, scaffold, initialize）、模块开发（develop module）。

  支持中英文任务描述。Use this skill for any coding task including: implementing features,
  building modules, creating projects, refactoring code, fixing bugs (non-trivial),
  optimizing performance, or making architectural changes. Always plan before coding.
version: v1.1
---

# 开发规划 Skill

## 核心原则

**任何编码任务都必须先规划，再动手。** 跳过规划直接写代码是不允许的。

---

## 第一步：主动提问（≥ 3 个，强制执行）

在开始规划之前，必须向用户提问，从以下维度中**选出最相关的 3-5 个问题**，覆盖至少 3 个维度：

### 需求维度（选 1-2 个）

- 这个功能/模块的**核心目标**是什么？解决什么问题？
- 预期的**使用方**是谁（终端用户、其他系统、定时任务、管理员…）？
- 功能的**输入和输出**是什么？有哪些边界或异常情况需要处理？
- 如何判断开发**完成**？有没有可验证的验收标准或示例用例？

### 技术维度（选 1-2 个）

- 当前项目使用的**技术栈和框架版本**是什么？（如项目结构明确则可跳过）
- 项目中是否有**现成模块/工具类/公共服务**可以复用？
- 是否有**性能、并发、安全**方面的特殊要求（QPS 目标、数据体量、鉴权机制等）？
- 是否需要对接**外部接口或第三方服务**？有没有沙箱环境？

### 约束维度（选 1-2 个）

- 是否需要**向后兼容**已有接口或数据库结构？
- 是否有需要遵守的**编码规范、分支规范或部署流程**？
- 是否存在**禁区**（不能改动的文件、不允许引入的依赖、不能修改的接口）？

### AI / ML 专项维度（仅检测到 AI/ML 项目时选 1-2 个）

- 使用的**模型或算法**是什么？是微调、推理还是训练流程？
- **数据集**从哪里来？需要预处理或清洗吗？数据量大概多少？
- 如何评估模型效果？有没有基准指标（Accuracy、F1、latency…）？
- 是否需要**实验追踪**（MLflow、W&B 等）？

> **提问方式**：将选出的问题整合成一段自然的对话，一次性向用户提出，
> 不要逐个追问，也不要开始写任何代码。等待用户回答后再进入第二步。
>
> **快捷判断**：如果用户的描述已经覆盖了某个维度（例如已提到技术栈），则跳过该维度的提问，避免重复。

---

## 第二步：检测项目类型，确定规划文件路径

根据项目根目录中存在的文件自动判断，**按优先级从高到低匹配**：

| 优先级 | 判断依据 | 项目类型 | 规划文件路径 |
|--------|---------|---------|------------|
| 1 | `pom.xml` 存在 | Java / Maven | `docs/dev-plan.md` |
| 2 | `build.gradle` 或 `build.gradle.kts` 存在 | Java / Gradle 或 Android | `docs/dev-plan.md` |
| 3 | `src/main/java` 目录存在 | Spring Boot | `docs/dev-plan.md` |
| 4 | `pubspec.yaml` 存在 | Dart / Flutter | `docs/dev-plan.md` |
| 5 | `*.xcodeproj` 或 `*.xcworkspace` 目录存在 | iOS / Swift / Xcode | `docs/dev-plan.md` |
| 6 | `Gemfile` 存在 | Ruby / Rails | `docs/dev-plan.md` |
| 7 | `composer.json` 存在 | PHP / Laravel | `docs/dev-plan.md` |
| 8 | `*.csproj` 或 `*.sln` 存在 | .NET / C# | `docs/dev-plan.md` |
| 9 | `pyproject.toml` 存在，或 `requirements.txt` 中含 `torch`/`tensorflow`/`transformers` | Python / AI·ML | `docs/dev-plan.md` |
| 10 | `pyproject.toml` 或 `requirements.txt` 存在（普通 Python） | Python | `docs/dev-plan.md` |
| 11 | `go.mod` 存在 | Go | `docs/dev-plan.md` |
| 12 | `Cargo.toml` 存在 | Rust | `docs/dev-plan.md` |
| 13 | `tsconfig.json` 存在且无 `package.json` | 纯 TypeScript | `DEV_PLAN.md` |
| 14 | `package.json` 且含 `react-native` 依赖 | React Native | `DEV_PLAN.md` |
| 15 | `package.json` 且含 `vue` 或 `nuxt` 依赖 | Vue / Nuxt | `DEV_PLAN.md` |
| 16 | `package.json` 且含 `next` 依赖 | Next.js | `DEV_PLAN.md` |
| 17 | `package.json` 且 `src/` 目录存在 | Node.js / 前端 | `DEV_PLAN.md` |
| 18 | `index.html` 或根目录有 `.html` 文件 | 纯前端 | `DEV_PLAN.md` |
| 19 | 根目录存在多个不同语言的特征文件（Monorepo） | Monorepo | `.claude/dev-plan.md` |
| 20 | 以上均不匹配 | 通用 | `.claude/dev-plan.md` |

**特殊情况：**

- **Monorepo**：若根目录同时存在多种语言特征文件（如同时有 `pom.xml` 和 `package.json`），说明是多语言 Monorepo，规划文件放 `.claude/dev-plan.md`，并在规划中注明影响的子项目
- **AI/ML 项目**：检测到 `requirements.txt` 含有 `torch`、`tensorflow`、`transformers`、`scikit-learn`、`keras`、`jax` 等 ML 依赖时，规划中需额外关注模型版本、GPU 资源、数据集处理等
- 若 `docs/` 目录不存在，**自动创建**
- 若规划文件已存在，在文件末尾**追加新任务规划**（保留历史记录，不覆盖）

---

## 第三步：生成并写入开发规划文件

按以下模板生成规划内容，**所有章节都必须填写**，不留空白占位符：

```markdown
# 开发规划：[任务标题]

> 创建时间：[YYYY-MM-DD HH:MM]
> 项目类型：[Java/Python/Flutter/…]
> 状态：规划中

---

## 一、需求分析

### 背景与目标
[用一段话说清楚：为什么要做这件事，要解决什么问题，成功后能带来什么价值]

### 功能范围
**本次在范围内：**
- [具体要实现的功能点 1]
- [具体要实现的功能点 2]

**明确不在范围内（避免范围蔓延）：**
- [排除的内容 1，说明为什么排除或何时做]

### 验收标准
- [ ] [可用自动化测试或手工验证的完成条件 1]
- [ ] [可验证的完成条件 2]
- [ ] [可验证的完成条件 3]

---

## 二、技术方案

### 架构概览
[用文字或简单 ASCII 图描述整体架构，说明各层/模块之间的调用关系]

示例（可按实际情况调整）：
```
请求 → Controller → Service → Repository → DB
                 ↓
           ExternalAPI
```

### 技术选型
| 方面 | 选择 | 理由 |
|------|------|------|
| [框架/库] | [具体选择] | [一句话理由] |
| [数据库/缓存] | [具体选择] | [一句话理由] |

### 模块 / 类 / 接口设计
[列出将要新建或修改的关键文件/类/函数，说明各自的职责]

| 文件 / 类 | 职责 | 新建/修改 |
|----------|------|---------|
| `src/...` | [职责说明] | 新建 |

### 关键接口 / 数据结构
```
[示例：REST API 路径、函数签名、数据模型字段、数据库表结构等]
```

### 依赖清单
[列出需要新增的第三方依赖，方便提前评估兼容性]

| 依赖名称 | 版本 | 用途 |
|---------|------|------|
| [library] | [version] | [用途] |

---

## 三、任务拆分

按照从小到大、可独立完成的原则拆分，每个子任务建议 1-4 小时内完成：

- [ ] **T1**：[任务名称] — [具体做什么，一句话说清楚]
- [ ] **T2**：[任务名称] — [具体内容]
- [ ] **T3**：[任务名称] — [具体内容]
- [ ] **T4**：[任务名称] — [具体内容]（如有）
- [ ] **T5**：单元测试 / 集成测试 / 联调验收

---

## 四、环境与部署

[说明开发/测试/生产环境的差异，以及本次改动的部署步骤]

- **本地开发**：[启动命令、所需环境变量、Mock 配置等]
- **测试环境**：[部署方式、接口地址、注意事项]
- **数据库变更**：[是否有 migration，如何执行]
- **配置变更**：[新增/修改的配置项名称及默认值]

> 若无环境差异或部署要求，此章节可简写为"无特殊部署步骤"。

---

## 五、风险与注意事项

| 风险点 | 可能影响 | 应对措施 |
|--------|---------|---------|
| [风险 1] | [影响范围] | [如何规避或处理] |
| [风险 2] | [影响范围] | [如何规避或处理] |

**编码注意事项：**
- [需要特别注意的技术细节或已知陷阱]
- [需要遵守的规范或约定]

---

*规划完成，即将开始执行 T1...*
```

---

## 第四步：规划完成后，再开始编码

规划文件写入完成后，告知用户：
1. 规划文件的路径
2. 本次任务的任务列表摘要（T1、T2...）
3. 即将开始执行哪个任务

然后按任务列表**逐项**推进开发，每完成一个任务，在规划文件中将对应复选框打勾 `[x]`。

---

## 特殊情况处理

| 情况 | 处理方式 |
|------|---------|
| 用户说"直接写代码，不用规划" | 提醒用户规划的价值，但尊重用户决定，记录为跳过规划 |
| 任务极小（如修改一个字符串、改一个配置项） | 跳过规划，直接执行，但需确认任务范围 |
| 规划文件已存在且当前任务有历史规划 | 新规划追加在文件末尾，用 `---` 分隔 |
| 用户在规划阶段修改了需求 | 更新规划文件后再开始编码 |
| 用户用英文描述任务 | 用英文提问和规划，规划文件语言跟随用户语言 |
| 任务跨越多个子项目（Monorepo） | 在规划中明确列出每个子项目受影响的范围和改动顺序 |

---

## 规划示例

下面是一个完整的规划示例，供参考输出质量和格式：

> **用户任务**："帮我在 Spring Boot 项目里实现用户登录功能，用 JWT 鉴权"

**第一步 — 提问示例：**

> 好的，在开始规划之前我有几个问题：
>
> 1. **需求**：登录支持哪种方式？仅用户名+密码，还是还需要手机验证码 / 第三方登录（微信、Google）？登录失败几次后是否需要锁定账号？
> 2. **技术**：项目目前有没有 Spring Security 依赖？用户信息存在哪张表？密码是否已经哈希存储？
> 3. **约束**：JWT 的过期时间有没有要求？是否需要支持 token 刷新？是否有接口需要公开访问（不鉴权）？

**生成的规划文件（`docs/dev-plan.md`）示例：**

```markdown
# 开发规划：用户登录（JWT 鉴权）

> 创建时间：2025-08-01 14:30
> 项目类型：Spring Boot / Java
> 状态：规划中

---

## 一、需求分析

### 背景与目标
当前系统无鉴权机制，所有接口均对外开放，存在安全风险。本次实现用户名+密码登录，颁发 JWT token，
后续接口通过 token 进行身份验证，实现接口访问控制。

### 功能范围
**本次在范围内：**
- 登录接口 POST /api/auth/login，返回 access_token（有效期 2h）
- JWT filter 对所有非白名单接口做 token 校验
- 密码使用 BCrypt 哈希比对

**明确不在范围内：**
- 手机验证码 / 第三方登录（下期迭代）
- token 刷新（refresh_token）机制（下期迭代）
- 登录失败锁定（后续需求）

### 验收标准
- [ ] 正确的用户名+密码可以换到 JWT token
- [ ] 携带有效 token 可以访问受保护接口
- [ ] 不带 token 或 token 过期返回 401
- [ ] 错误密码返回 400 并不泄露用户是否存在

---

## 二、技术方案

### 架构概览
```
POST /api/auth/login
     ↓
AuthController.login()
     ↓
UserService.authenticate()  →  UserRepository (查用户)
     ↓
JwtUtil.generateToken()
     ↓
返回 { access_token, expires_in }

每次请求:
HTTP Request → JwtAuthFilter → (解析token) → SecurityContext → Controller
```

### 技术选型
| 方面 | 选择 | 理由 |
|------|------|------|
| JWT 库 | `io.jsonwebtoken:jjwt` 0.11.5 | 社区主流，项目已有依赖 |
| 密码哈希 | Spring Security BCrypt | 框架内置，无需额外依赖 |
| 鉴权框架 | Spring Security | 与 Spring Boot 深度集成 |

### 模块 / 类 / 接口设计
| 文件 / 类 | 职责 | 新建/修改 |
|----------|------|---------|
| `AuthController` | 提供 /login 端点 | 新建 |
| `JwtUtil` | token 生成与解析 | 新建 |
| `JwtAuthFilter` | 请求拦截、token 校验 | 新建 |
| `SecurityConfig` | 配置白名单、filter 顺序 | 新建 |
| `UserService` | 增加 authenticate 方法 | 修改 |

### 关键接口 / 数据结构
```
POST /api/auth/login
Body: { "username": "string", "password": "string" }
200:  { "access_token": "eyJ...", "expires_in": 7200 }
400:  { "code": "INVALID_CREDENTIALS", "message": "用户名或密码错误" }

JWT Payload: { "sub": "userId", "username": "xxx", "exp": 1234567890 }
```

### 依赖清单
| 依赖名称 | 版本 | 用途 |
|---------|------|------|
| `jjwt-api` | 0.11.5 | JWT 生成与解析 |
| `spring-boot-starter-security` | 与 Boot 版本对齐 | 鉴权框架 |

---

## 三、任务拆分

- [ ] **T1**：添加依赖，创建 `JwtUtil`（生成 / 解析 token）
- [ ] **T2**：实现 `AuthController` + `UserService.authenticate()`
- [ ] **T3**：实现 `JwtAuthFilter`，注册到 SecurityConfig
- [ ] **T4**：配置白名单（/api/auth/login 等无需鉴权）
- [ ] **T5**：单元测试（JwtUtil、authenticate 方法）+ Postman 联调验收

---

## 四、环境与部署

- **本地开发**：application.yml 新增 `jwt.secret` 和 `jwt.expiration`（本地用任意字符串）
- **测试/生产环境**：`jwt.secret` 必须设为强随机字符串，通过环境变量注入，不提交到 git
- **数据库变更**：无
- **配置变更**：`jwt.secret`（必填）、`jwt.expiration`（默认 7200 秒）

---

## 五、风险与注意事项

| 风险点 | 可能影响 | 应对措施 |
|--------|---------|---------|
| jwt.secret 提交到 git | 安全漏洞 | .gitignore 排除本地配置，CI 用 secret 注入 |
| 现有接口被 Security 拦截 | 所有接口 401 | 白名单配置优先验证，上线前全接口回归 |

**编码注意事项：**
- `UserService.authenticate()` 中只抛 `BadCredentialsException`，不区分"用户不存在"和"密码错误"，避免用户枚举
- JWT secret 长度建议 ≥ 256 bit（32字节）

---

*规划完成，即将开始执行 T1...*
```
