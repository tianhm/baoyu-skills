---
name: xhs-images
description: 小红书 (Xiaohongshu/RedNote) infographic series generator. Breaks down content into 1-10 cartoon-style infographics, generates outline and creates images using gemini-web skill.
---

# 小红书 Infographic Series Generator

Break down complex content into eye-catching cartoon-style infographic series for Xiaohongshu (Little Red Book).

## Role

Visual content strategist specializing in breaking down complex content into engaging cartoon-style infographic series.

## Usage

```bash
# Option 1: Specify article path
/xhs-images posts/ai-future/article.md

# Option 2: Direct content input
/xhs-images
[paste content]
```

## File Management

### With Article Path

Save to `xhs-images/` subdirectory in the same folder as the article:

```
posts/ai-future/
├── article.md
└── xhs-images/
    ├── outline.md
    ├── prompts/
    │   ├── 01-cover.md
    │   ├── 02-content-1.md
    │   └── ...
    ├── 01-cover.png
    ├── 02-content-1.png
    └── 03-ending.png
```

### Without Article Path

Save to `xhs-outputs/YYYY-MM-DD/[topic-slug]/`:

```
xhs-outputs/
└── 2026-01-08/
    └── ai-agent-guide/
        ├── outline.md
        ├── prompts/
        │   ├── 01-cover.md
        │   └── ...
        ├── 01-cover.png
        └── 02-ending.png
```

- `[topic-slug]`: lowercase English with hyphens based on topic
- **Conflict handling**: If slug directory exists, generate a different slug; never overwrite
- Same day + same topic → same directory

## Workflow

### Step 1: Analyze Content & Determine Image Count

| Content Type | Image Count |
|-------------|-------------|
| Simple opinion / single topic | 2-3 |
| Medium complexity / tutorial | 4-6 |
| Deep dive / multi-dimensional | 7-10 |

### Step 2: Generate Outline

Plan for each image:
- Position (cover / content / ending)
- Core message (one sentence)
- Text content (title, subtitle, key points)
- Visual prompt

**Outline Format:**

```markdown
# 小红书 Infographic Series Outline

**Topic**: [topic description]
**Image Count**: N
**Generated**: YYYY-MM-DD HH:mm

---

## Image 1 of N

**Position**: Cover
**Core Message**: [one-liner]
**Filename**: 01-cover.png

**Text Content**:
- Title: xxx
- Subtitle: xxx

**Visual Prompt** (image-specific only, system.md provides base style):
```
小红书风格信息图，竖版（3:4），卡通风格，手绘风格文字，[具体背景色]背景。
[具体内容布局描述]
文字内容：...
卡通元素：...
整体风格：手绘、可爱、清新，信息精简，多留白，重点突出。
```

---

## Image 2 of N
...
```

### Step 3: Save Outline

Save outline as `outline.md`.

### Step 4: Generate Images One by One

For each image in the outline:

1. **Save prompt file** to `prompts/` subdirectory (filename matches image, .md extension):

```markdown
小红书风格信息图，竖版（3:4），卡通风格，手绘风格文字，[background color]背景。

[Layout description: title position, text arrangement, visual hierarchy]

文字内容：
- 主标题：「xxx」（大号手绘字体，居中/顶部）
- 副标题/要点：「xxx」「xxx」

卡通元素：[specific elements: lightbulb icons, speech bubbles, arrows, emoji-style expressions]

整体风格：手绘、可爱、清新，信息精简，多留白，重点突出。
```

2. **Generate image using `/gemini-web` skill**:

```
/gemini-web --promptfiles [SKILL_ROOT]/skills/xhs-images/prompts/system.md [TARGET_DIR]/prompts/01-cover.md --image [TARGET_DIR]/01-cover.png
```

- `[SKILL_ROOT]`: The baoyu-skills root directory
- `--promptfiles`: Concatenates system.md (style guidelines) + image-specific prompt
- `--image`: Output path for generated image

After each image:
1. Confirm generation success
2. Report progress: "Generated X/N"
3. Continue to next

### Step 5: Completion Report

```
小红书 Infographic Series Complete!

Topic: [topic]
Location: [directory path]
Images: N total

- 01-cover.png ✓ Cover
- 02-content-1.png ✓ Content
- 03-content-2.png ✓ Content
- 04-ending.png ✓ Ending

Outline: outline.md
```

## Content Breakdown Principles

1. **Cover (Image 1)**: Strong visual impact, core title, attention hook
2. **Content (Middle)**: One core point per image, moderate information density
3. **Ending (Last)**: Summary / call-to-action / memorable quote

## Visual Style Guidelines

- **Type**: Infographic
- **Orientation**: Portrait, 3:4
- **Style**: Cartoon, hand-drawn
- **Background**: Morandi colors / cream / off-white / soft tones
- **Text**: Must be hand-drawn style, no realistic fonts
- **Decoration**: Simple cartoon elements, icons for visual interest
- **Layout**: Concise info, ample whitespace, clear hierarchy

## Terminology

- Token → Token
- AI Agent → AI 智能体
- Vibe Coding → 凭感觉编程
- AI Wrapper → AI 套壳

## Notes

- Image generation typically takes 10-30 seconds per image
- Auto-retry once on generation failure
- Use cartoon alternatives for sensitive public figures
- Output language matches input content language
