# 代码仓库分析（prompts.chat）

本文档简要分析项目定位、技术栈、架构、数据模型与可扩展性设计。

## 1. 项目定位

- 这是一个围绕 AI Prompt 的社区平台，支持分享、发现、收藏与协作编辑。
- 同时支持自托管和白标配置（品牌、主题、功能开关、认证方式）。

## 2. 核心技术

- 前端与全栈框架：Next.js（App Router）+ React 19 + TypeScript。
- 数据层：Prisma + PostgreSQL。
- 认证：NextAuth（通过插件化 provider 动态装配）。
- 国际化：next-intl（多语言）。

## 3. 架构特点

- **配置驱动**：`prompts.config.ts` 提供品牌、主题、认证、功能开关。
- **运行时覆盖**：`src/lib/config/index.ts` 支持 `PCHAT_*` 环境变量覆盖，实现无需重建的部署定制。
- **插件化**：`src/lib/plugins/*` 将认证与存储以插件注册机制解耦。
- **路由清晰**：App Router 下按业务域拆分（prompts、tags、categories、auth、api 等）。

## 4. 数据模型亮点

- `User`、`Prompt`、`PromptVersion`、`ChangeRequest` 等模型体现“内容协作+版本化”能力。
- `Prompt` 具备 `isPrivate`、`isUnlisted`、`deletedAt`、`featuredAt`、`embedding` 等字段，支持可见性、运营和 AI 语义能力。
- 多对多关联完整：Tag、Vote、Collection、Contributors、Connections 等为社区互动提供基础。

## 5. 工程化与质量

- npm scripts 覆盖构建、lint、测试、Prisma 生命周期。
- 使用 ESLint、Vitest、TypeScript strict 风格进行约束。
- 代码中普遍采用 server component + API route 的组合，易于扩展。

## 6. 适合的演进方向

- 补齐端到端测试（Playwright）和关键 API 合约测试。
- 将高流量查询（发现页、热门提示）引入缓存层。
- 对 Prompt 检索与推荐增加异步索引/队列化处理。
