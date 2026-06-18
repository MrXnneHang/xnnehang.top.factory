---
name: xnne-factory
description: Xnne's Obsidian writing factory architecture. Documents the real relationship between xnnehang.top.factory (Obsidian Vault) and nyakku.moe (Fuwari blog), image path conventions, and the shared image-hosting submodule workflow.
---

# Xnne Factory Architecture

## 两个站点的真实关系

| 站点 | 角色 | 系统 | 维护方式 |
|---|---|---|---|
| **xnnehang.top.factory** | 写作道场 | Obsidian + YOLO | 日常写作发生地，Korewaxnne 作业区 |
| **nyakku.moe (xnnehang.top)** | 生产博客 | Fuwari (Astro) | 手动 post，Korewaxnne 不直接参与 |

- 两者内容**同源但不完全相同**，主要区别在于图片路径、少量内容剪裁
- 用户只需要在 Obsidian 道场写作、修改；post 部署由用户手动完成
- Korewaxnne 的工作范围仅限于 Obsidian 道场，不操作 Fuwari 仓库

## 图片路径规范

这是两个站点最主要的差异点：

| 环境 | 相对路径 | 从何出发 | 说明 |
|---|---|---|---|
| **Obsidian 道场** | `assets/img/...` | vault root（`xnnehang.top.factory/`） | 本地预览可用 |
| **Fuwari 博客** | `../../assets/img/...` | `src/content/posts/xxx.md` | 构建后可访问 |

### 转换规则

从 Obsidian 道场搬运文章到 Fuwari 时：
1. 图片路径从 `assets/img/...` 改回 `../../assets/img/...`
2. 其他内容不变

> [!note]
> 虽然每次搬运需要改路径，但 Obsidian 的日常写作体验和 Fuwari 的生产环境互不干扰，这是有意为之的取舍。

## image-hosting submodule

- **远程仓库**: `MrXnneHang/image-hosting`（GitHub）
- **在 Obsidian 道场的挂载点**: `assets/img/`
- **在 Fuwari 项目的挂载点**: `src/assets/img/`
- 两边共享同一份图片资源，各自在本地的相对路径不同

### 维护说明

向 `assets/img/` 添加新图片后：
```bash
cd xnnehang.top.factory
git submodule update --remote  # 拉取远端更新
# 如果有本地新增图片
cp <新图片> assets/img/covers/
cd assets/img
git add . && git commit -m "add new cover"
git push
cd ..
git add assets/img && git commit -m "update image-hosting submodule"
```

## 工作流总览

```
写作（Obsidian 道场）          部署（手动）
─────────────────           ──────────
1. 在 vault 写 markdown        4. 搬运到 Fuwari 仓库
2. Korewaxnne 协助润色         5. 改图片路径
3. 图片放 assets/img/          6. `pnpm build && deploy`
```

## 与 xnne-blog skill 的关系

- `xnne-core` 定义身份和 voice（与仓库无关）
- `xnne-blog` 定义博客写作流程（两个仓库通用）
- `xnne-factory` 定义仓库架构和路径规范（区分两个仓库）

写作时加载 `xnne-blog` → 自动加载 `xnne-core`；需要确认路径时参考 `xnne-factory`。
