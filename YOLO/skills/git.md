---
name: git
description: Git workflow assistant for xnnehang.top.factory vault. Use when the user requests git operations (commit, push, pull, status, log, branch, submodule), asks about git workflows, or needs repo maintenance. Covers Xnne's conventions (中文 commits, main-only) and Windows PowerShell quirks.
---

# Git Workflow (xnnehang.top.factory)

## Conventions

- **Branch**: main only（不建 feature branch）
- **Commit language**: 中文
- **Commit format**: `<type>: <描述>`

### Allowed Types

| Type | 使用场景 | 示例 |
|---|---|---|
| `feat` | 新内容 / 新笔记 | `feat: 添加芙莉莲观后感` |
| `fix` | 修复 | `fix: 修复图片路径` |
| `docs` | 文档 / 配置说明 | `docs: 更新 README` |
| `refactor` | 重构 | `refactor: 重组 YOLO 目录结构` |
| `chore` | 配置 / 依赖 / 杂务 | `chore: 更新 .gitignore` |
| `submodule` | 子模块更新 | `submodule: 新封面图片` |

## Repository Structure

```
xnnehang.top.factory/          # 父仓库（此 vault）
├── .git/
├── .gitignore
├── .gitmodules
├── assets/img/                # git submodule → MrXnneHang/image-hosting
├── YOLO/
│   ├── skills/
│   └── ...
├── *.md (笔记文件)
└── ...
```

**重要**: `assets/img/` 是独立的子模块仓库。父仓库只追踪它的 commit 指针，不追踪内部文件。

## 安全规则（必须遵守）

1. **每次执行前确认**：`git push`、`git pull`、`git reset`、`git checkout --`、`git revert`、`git clean`、`git merge`、`git rebase`
2. **必须问清楚范围再 stage**：不要替用户决定 stage 哪些文件
3. **子模块优先**：如果 `assets/img/` 有改动，先提交子模块再提交父仓库
4. **不要 force push**：除非用户明确要求
5. **不要自动推送**：commit 后先告知用户，等确认再 push

## 命令参考

### 基础操作

```powershell
# 查看状态
git status

# 查看精简状态
git status --short

# 暂存单个文件（用相对路径）
git add <file>

# 暂存所有已跟踪文件的改动
git add -u

# 提交
git commit -m "feat: 描述"

# 推送
git push

# 拉取
git pull

# 查看最近提交
git log --oneline -10

# 查看改动
git diff
git diff --cached   # 已暂存的改动

# 撤销未暂存的改动（⚠️ 危险，先确认）
git checkout -- <file>
```

### 子模块操作

```powershell
# 拉取子模块远端更新
git submodule update --remote
git add assets/img
git commit -m "submodule: 同步远端图片更新"

# 子模块内有本地新图片时（两步提交）
# Step 1: 在子模块内提交
Set-Location assets/img
git add .
git commit -m "chore: 添加新图片"
git push
Set-Location ..

# Step 2: 在父仓库更新子模块指针
git add assets/img
git commit -m "submodule: 更新图片子模块"
```

> **注意**：`Set-Location assets/img` 是 PowerShell 的 `cd`。每次 `terminal_command` 都是新会话，所以子模块的多步操作需要放在同一次调用中（用 `;` 分隔），或用后台会话。

### 查看配置

```powershell
git remote -v
git config user.name
git config user.email
```

## Windows PowerShell 注意事项

1. **没有 `&&` 或 `||`**：PowerShell 不支持类 Unix 的命令链操作符。用 `;` 分隔或分批执行。
2. **编码问题**：`git status` / `git log` 输出中的中文可能在终端显示乱码，但不影响功能。
3. **路径风格**：统一用 `/` 作为路径分隔符（git 和 PowerShell 都认识）。
4. **命令被拦截**：部分 git 命令可能被 YOLO 终端工具的安全策略拦截。遇到时：
   - 告诉用户具体哪个命令被阻止
   - 提供完整的命令文本，让用户在终端手动执行
   - 如果是安全操作（如 `git status`），可以尝试用 `Test-Path` 等 PowerShell 原生命令替代

## 工作流示例

### 日常写作后提交

```
用户: "帮我提交新写的笔记"
Korewaxnne: 检查 git status → 确认要 stage 的文件 → git add → git commit
```

### 添加新图片后

图片涉及**两个仓库**（子模块存放文件、父仓库追踪指针）和**一个引用一致性检查**。

**完整流程：**
1. 确认图片放到了正确的位置（一般按文章 slug 分文件夹：`assets/img/<post-slug>/` 或 `assets/img/covers/`）
2. **检查 markdown 文件中的引用路径**：粘贴图片到 Obsidian 时自动生成的路径可能与最终路径不一致（例如 Obsidian 默认附件文件夹可能不是 `assets/img/`）。必须确保 `![alt](路径)` 里的路径指向实际存放位置
3. 如果移动了图片位置——更新所有引用该图片的 markdown 文件中的路径
4. 进入子模块提交新图片：
   ```
   Set-Location assets/img
   git add .
   git commit -m "chore: 添加新图片"
   git push
   Set-Location ..
   ```
5. 回到父仓库更新子模块指针：
   ```
   git add assets/img
   git commit -m "submodule: 更新图片子模块"
   ```

```
用户: "我放了新封面图"
Korewaxnne: 确认图片存放位置 → 检查 markdown 引用路径是否一致 → 子模块 add+commit+push → 父仓库更新子模块指针
```

### 同步远端变更

```
用户: "帮我拉取最新"
Korewaxnne: 先 pull 父仓库 → 再 submodule update --remote
```

## .gitignore 策略说明

父仓库的 `.gitignore` 会忽略根目录下的图片文件（`*.png`, `*.jpg` 等），因为正规图片都应放在 `assets/img/` 子模块中。写作过程中误粘贴到根目录的图片不会意外进入版本控制。
