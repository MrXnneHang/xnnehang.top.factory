# xnnehang.top.factory

Xnne 的 Obsidian 写作道场。

## 这是什么

这是我的写作胚房。所有博客文章最先在这里写、改、存档，然后搬运到 [nyakku.moe (xnnehang.top)](https://xnnehang.top) 发布。

```
xnnehang.top.factory/          ← 本仓库（Obsidian 道场）
├── assets/img/                ← image-hosting submodule
├── *.md                       ← 博客源文件
├── YOLO/                      ← Korewaxnne 作业区
│   ├── skills/                ← AI 写作技能定义
│   ├── memory/                ← 全局记忆
│   └── snippets.md            ← 快捷指令
└── README.md
```

## 与 nyakku.moe 的关系

| | Obsidian 道场（本仓库） | Fuwari 博客（生产环境） |
|---|---|---|
| 角色 | 写作、编辑、存档 | 发布、展示 |
| 图片路径 | `assets/img/...` | `../../assets/img/...` |
| 维护者 | Xnne + Korewaxnne | Xnne 手动搬运 |

图片路径不同是设计使然：Obsidian 需要本地可预览，Fuwari 有自己从 `src/content/posts/` 出发的目录结构。搬运时记得把路径改回来。

## 图片资源

图片托管在独立仓库 [MrXnneHang/image-hosting](https://github.com/MrXnneHang/image-hosting)，通过 Git submodule 挂载到 `assets/img/`。

## 目录分类

博客文章按以下分类组织：

- **观后** — 回忆展柜与同类诱捕器
- **思考** — 从具体事物出发的深层反思
- **教程** — 流程类记录
- **边写边学** — 探索过程中的笔记
- **资源** — 渠道、应用、信息推荐

---

> 写于 2026-06-18 · 由 [Korewaxnne](https://github.com/xnne-bot) 协助整理
