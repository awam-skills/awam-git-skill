---
name: awam-git
description: >-
  为仓库安装 Awam 团队 Git 提交规范（Conventional Commits + 中文提交信息），写入
  `.cursor/rules/git-commit.mdc` 与根目录 `.cursorrules`。在用户提到 awam-git、
  提交规范、Conventional Commits、中文 commit、仿照 awam 项目约定，或要求给项目
  配置 Cursor/Agent / Source Control 生成提交信息规则时使用。
disable-model-invocation: true
---

# awam-git

为当前 Git 仓库安装 Awam 提交规范，使 **Cursor Agent** 与 **Source Control ✨ 生成提交信息** 都遵循同一约定。

## 关键：两套文件缺一不可

| 文件 | 谁读取 | 作用 |
|------|--------|------|
| `.cursor/rules/git-commit.mdc` | Cursor Agent（`alwaysApply: true`） | Agent 写/执行 commit 时遵守 |
| `.cursorrules`（仓库根目录） | Source Control ✨「生成提交信息」 | UI 自动生成 commit message 时遵守 |

**常见问题**：只写了 `.cursor/rules` 时，Agent 会用中文，但左侧 ✨ 仍生成英文——因为 ✨ **不会**读 `.cursor/rules`。

## 何时执行

用户要求：安装/配置提交规范、awam-git、中文 Conventional Commits、Source Control 生成英文要改成中文、或仿照 awam 项目约定。

## 安装步骤

1. **确认目标仓库根目录**（含 `.git`）。多根工作区时：用户点名的仓库优先；否则对每个含 `.git` 的根分别安装；勿装到子目录。
2. **快速扫结构**（顶层目录、`apps/`、`packages/`、`src/` 等），归纳 **2–7 个**常用 `scope`（用真实模块/目录名，见下方对照表）。
3. **读取模板并替换占位符后写入**（见 [templates/](templates/)）：
   - [templates/git-commit.mdc](templates/git-commit.mdc) → `.cursor/rules/git-commit.mdc`
   - [templates/cursorrules](templates/cursorrules) → `.cursorrules`
4. **替换规则**：把模板中所有 `<scope-*>`、示例中文简述，改成当前仓库真实 scope 与业务相关示例（至少 3 条 `feat`/`fix`/`docs` 或 `refactor` 示例）。
5. **已存在文件**：默认按模板**全面覆盖**为 Awam 规范（用户明确要求保留旧内容时再合并，且必须保留：中文 subject、Conventional Commits、`.cursorrules` 中的 MUST/NEVER 英文约束句）。
6. **校验清单全部勾过**后，简要告知用户已安装路径、选用的 scope、以及「✨ 仍英文时请 Reload Window」。
7. **不要**主动 `git commit` / `git push`，除非用户明确要求。

## 模板占位符

| 占位符 | 替换为 |
|--------|--------|
| `<scope-1>` … `<scope-N>` | 本仓库真实 scope（如 `room`、`auth`、`front`） |
| `<中文…简述>` | 与该仓库业务相关的中文示例 |

`.cursorrules` 中保留英文 MUST/NEVER 句式（✨ 生成器对硬性英文约束更稳）；示例与 subject 必须是简体中文。

## scope 选择

| 项目形态 | scope 示例 |
|---------|-----------|
| 前端 monorepo | `lobby`、`share`、具体游戏名、`ui` |
| 游戏服务端 | `room`、`auth`、`games`、`users`、`cards`、`settings` |
| 通用 monorepo | `front`、`server`、`db`、`shared` |
| skills / 工具仓 | `skills`、具体 skill 名、`sync` |
| 单包应用 | `auth`、`api`、`ui`、功能域名 |

无合适 scope 时可省略：`docs: 补充本地开发环境说明`。文档用 type `docs` 或 `docs(<模块>)`，勿再设 `doc` scope；CI 用 type `ci`，勿 `chore(ci)`。

## 安装后规范摘要（写入文件时必须完整落地）

模板已含完整条文；安装时**不要删减**下列类别，只替换 scope/示例：

- 格式：`<type>[!][(scope)]: <中文简述>` + 可选正文/页脚 / BREAKING CHANGE（scope 与 `!` 均可省略）
- type 全表：feat / fix / docs / style / refactor / perf / test / build / ci / chore / revert
- 中文 subject：≤72 字、不以句号结尾；祈使语气；写「做什么」；动机放正文；禁止纯英文 subject；禁止含糊词
- type/scope 小写；一条提交一类变更；含 `revert` 写法
- 安全：不提交密钥；仅用户要求时 commit；不用 `--no-verify`；不擅自 push/amend/force
- Agent：HEREDOC 传 message；Source Control 依赖 `.cursorrules`

详细条文以模板为准，需要对照时再打开 [reference.md](reference.md)。

## 校验清单

- [ ] `.cursor/rules/git-commit.mdc` 存在且 frontmatter 含 `alwaysApply: true`
- [ ] 仓库根目录存在 `.cursorrules`（与 `.cursor/rules` 同级的上一级，即 repo root）
- [ ] 两处均为 Conventional Commits + **中文** subject 示例
- [ ] `.cursorrules` 含 MUST（中文 subject）与 NEVER（纯英文 subject）约束
- [ ] 示例 scope 与当前仓库相符；无残留 `<scope-1>` 占位符
- [ ] `git-commit.mdc` 含安全约束（密钥 / 仅显式 commit / 禁止擅自 `--no-verify`）
- [ ] 未主动创建 git commit

## 安装后用户提示（简短）

告知：

1. 已写入的两个路径  
2. 本仓库推荐 scope 列表  
3. 若 Source Control ✨ 仍出英文：命令面板执行 **Developer: Reload Window** 后重试
