# Awam Git 提交规范参考

安装时以 [templates/](templates/) 为准写入仓库；本文供对照，不必复制进目标仓库。

## 为何需要两个文件

| 通道 | 读取文件 |
|------|----------|
| Agent 对话里协助提交 | `.cursor/rules/git-commit.mdc`（`alwaysApply: true`） |
| Source Control ✨ 生成提交信息 | 仓库根目录 `.cursorrules` |

只配置其一会导致：Agent 中文、✨ 仍英文（或反之）。

## 完整格式

```
<type>[!][(scope)]: <中文简述>

[正文：动机 / 背景；与 subject 空一行；建议每行 ≤72 字]

[页脚：BREAKING CHANGE / Refs / Closes / Fixes]
```

- `type` / `scope` 小写；`scope` 与 `!` 均可省略
- subject：做什么（祈使：修复/新增/调整）；body：为什么
- CI 用 `ci`，勿 `chore(ci)`；文档用 `docs` / `docs(<模块>)`，勿 `docs(doc)`
- 回滚：`revert: <原简述>` + `This reverts commit <hash>.`

## type 速查

feat / fix / docs / style / refactor / perf / test / build / ci / chore / revert

## 质量标准

- 简述：简体中文，≤72 字，无句末句号，祈使语气
- 禁止纯英文 subject、含糊词、无关改动打包
- 可保留专有名词与代码标识
- 敏感文件不入 commit；仅用户要求时 commit/push；默认不用 `--no-verify`

## 多根工作区

对每个含 `.git` 的根分别安装；scope 按该仓库结构单独归纳，勿混用另一仓库的模块名。

## 安装后 ✨ 仍英文

1. 确认 `.cursorrules` 在**该 git 根目录**（不是父目录或子包）
2. Developer: Reload Window
3. 再点 ✨ 生成
