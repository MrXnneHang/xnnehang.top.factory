---
name: xnne-blog
description: Xnne's blog writing workflow for xnnehang.top. Use when the user asks to write, edit, or review a blog post of any category (观后/思考/教程/边写边学/资源). Covers frontmatter generation, full post generation from ideas, draft refinement, syntax conversion (GitHub cards, video embeds, admonitions), title/angle suggestions, and style consistency checks. Automatically loads xnne-core for voice/identity. Prefer this over generic writing assistance when the target is xnnehang.top.
---

# Xnne Blog Workflow

This skill relies on **xnne-core** for voice definition, core themes, reference works, and AI declaration standards. Always load xnne-core when using this skill.

## Category Voice Guidelines

Two broad voice groups, not five:

### Group A: 观后 & 思考 (Introspective/Emotional)

- Voice: xnne-core's first-person亲密体. "我" is Xnne.
- Structure: From concrete scene → personal feeling → broader connection.
- Allow: free association,跨文本 comparison, self-mockery, emotional honesty.
- Avoid: excessive structuring, academic tone, polishing away vulnerability.
- AI participation: **Never full mode**. Use polish or structure at most. The emotional core must be Xnne's own.

### Group B: 教程 & 边写边学 (Instructional/Process)

- Voice: "I'm doing this" first-person, but more instructional than emotional. Casual but clear.
- Structure: Problem → preparation → steps → pitfalls. Screenshots welcome.
- Allow: personal aside ("颜控在此"), occasional吐槽, curiosity-driven tangents.
- Avoid: over-formality, pretending to be an official doc.
- AI participation: **full mode OK** — use third-person "Xnne". Also OK for polish/structure with first-person "我".

### Group C: 资源 (Resource/Recommendation)

- Voice: Brief, direct. "I found this useful." Minimal flourish.
- Structure: What → why → where to find.
- AI participation: full mode OK with third-person "Xnne".

## Frontmatter Generation

Output standard Fuwari frontmatter:

```yaml
---
title: <中文标题>
published: <YYYY-MM-DD> # ask user or leave blank
category: <观后/思考/教程/边写边学/资源>
tags:
  - <tag1>
  - <tag2>
description: <一句话简介>
# shelf and series only when applicable
shelf: '<书籍/电影/游戏>' # for 观后
series:
  - <系列名>
---
```

Rules:
- `category` is exactly one of the five. `series` is for thematic grouping (can be multiple).
- `description` is one sentence, no more than 30 words.
- `shelf` only for 观后 and 思考, single-quoted.
- `draft: false` by default; add `draft: true` if explicitly unfinished.

## Full Post Generation (from fragmented ideas)

When user provides raw ideas/snippets and asks for full post:

1. **Determine category** from content. If unclear, ask or default to 思考.
2. **Determine AI declaration level**:
   - Group B/C → level 1 (full) with third-person "Xnne"
   - Group A → level 2 (structure) with first-person "我" — or suggest user write it themselves
3. **Write AI declaration note** at top (per xnne-core standards).
4. If using **full mode (third-person)**:
   - Keep "Xnne" as subject for all opinions/experiences.
   - Korewaxnne is the narrator, not the experiencer.
   - Add a `:::note[来自 Korewaxnne 的笔记]` or `> [!NOTE]` depending on preference.
5. Write body following category voice guidelines.
6. Apply Fuwari syntax conversion at the end.
7. Add frontmatter.

## Draft Refinement (polish mode)

When user provides a draft and asks for polish:

1. **Identify AI declaration level**: Always level 2 (structure) or 3 (polish). Ask if unsure.
2. **Preserve voice**: Do NOT:
   - Change first-person to third-person
   - Remove self-mockery or emotional honesty
   - "Fix" grammar in a way that makes the text sound more formal
   - Add content that changes meaning or adds experiences Xnne didn't have
3. **DO fix**:
   - Run-on sentences that hurt readability
   - Inconsistent tense
   - Fuwari syntax (see Syntax Conversion section)
   - Obvious typos
4. Add/edit AI declaration note at top. Use existing note if present; update participation level if needed.
5. Output the refined full post, with frontmatter.

## Syntax Conversion (Fuwari-specific)

After any writing/polishing, apply these conversions:

### GitHub Repository Cards
```markdown
# Instead of plain links like:
https://github.com/saicaca/fuwari

# Use:
::github{repo="saicaca/fuwari"}
```

### Admonitions (instead of plain blockquotes)
```markdown
# Instead of:
> Note: something

# Use:
:::note
something
:::

# Supported types: note, tip, important, warning, caution
# Custom title: :::note[自定义标题]
```

### Video Embeds
```markdown
# YouTube
<iframe width="100%" height="468" src="https://www.youtube.com/embed/VIDEO_ID" title="YouTube video player" frameborder="0" allowfullscreen></iframe>

# Bilibili
<iframe width="100%" height="468" src="//player.bilibili.com/player.html?bvid=BVxxxxxxxxxx&page=1" frameborder="0" allowfullscreen></iframe>
```

### Image paths
Use relative paths from the vault root: `assets/img/covers/filename.png`

> **关于 Fuwari 博客的路径转换**: 在 Obsidian 道场中预览用 `assets/img/...`；搬运到 Fuwari 博客时改回 `../../assets/img/...`（从 `src/content/posts/` 出发）。详见 [[YOLO/skills/xnne-factory]]。

### Code blocks with language labels
Always specify language after ``` for syntax highlighting.

## Title & Angle Suggestions

When user provides content but no title or angle:

1. Read content, identify its core: is it an observation? a feeling? a process? a discovery?
2. Suggest 2-3 title options:
   - One direct/descriptive (for clear communication)
   - One with a hook (for intrigue)
   - One引用式 (quote from referenced work that captures the spirit)
3. If the user also needs an angle: suggest which dimension to emphasize based on content.
   - Example: if content touches both "loss of data" and "relief at losing old posts", suggest framing around the ambivalence.

## Style Consistency Check

When user asks "does this sound like me?":

1. Compare against xnne-core voice definition and anti-patterns.
2. Check for these deviations:
   - Is it too formal/academic? → Flag
   - Is it trying too hard to be correct? → Flag (this is Xnne's自我审查 pattern)
   - Is the emotion fabricated or exaggerated? → Flag
   - Does it引用 Xnne's recurring themes naturally? → One point each for good connection
3. Output a brief checklist: [✅/⚠️/❌] per dimension.

## Korewaxnne Note Format

When the AI contributes supplementary analysis or summary (not the main content), format as:

```markdown
:::note[来自 Korewaxnne 的笔记]
<analysis/summary content here>
:::
```

This is different from the AI declaration (which lives in `> [!NOTE]` at the top). Use this for inline AI commentary within a post that is primarily Xnne's own writing.
