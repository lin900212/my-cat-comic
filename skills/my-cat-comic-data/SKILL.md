---
name: my-cat-comic-data
description: Use when the user wants to generate, update, or publish My Cat Comic `students.json` data from story sheets, class rosters, role assignments, or student character images for the AI Warriors / My Cat Comic GitHub Pages site.
---

# My Cat Comic Data

## Purpose

Generate and maintain `students.json` for the My Cat Comic task website.

Use this skill when the user asks for any of these:

- 依照故事單和學生名單產生 My Cat Comic 學生資料
- 產生或更新 `students.json`
- 根據學生扮演角色，重述故事成「我的四個事件」
- 把新的 `students.json` 上傳到 GitHub Pages
- 幫某班、某組、某幾位學生建立 My Cat Comic 任務資料

## Current Site

Default repo:

```text
/Users/chenguanlin/code/paste/my-cat-comic-site
https://github.com/lin900212/my-cat-comic
https://lin900212.github.io/my-cat-comic/
```

Default data file:

```text
/Users/chenguanlin/code/paste/my-cat-comic-site/students.json
```

Default image folder:

```text
/Users/chenguanlin/code/paste/my-cat-comic-site/images/
```

## Input To Ask For

If missing, ask only for the minimum needed:

```text
1. 故事單或故事摘要
2. 學生名單
3. 每位學生的班級/組別
4. 每位學生扮演的角色
5. 角色圖檔名或圖片路徑，如果已有
```

If the user provides only a class roster without role assignments, create examples only when they explicitly ask for examples. State any role assumptions clearly, such as:

```text
我先假設前三位依序是 Narrator、Coach、Knight。
```

## Student Image Matching

The user does not need to rename student role images before sending them.

When the user uploads a batch of role images with unclear filenames, use one of these matching methods:

### Ordered Matching

If the user says the images are in roster or `students.json` order, match them directly by order.

Example user instruction:

```text
這些圖片依序對應 students.json 裡風班學生順序。
請重新命名圖片、放到 images/、更新 students.json，commit 並 push。
```

Process:

1. Read the current `students.json`.
2. Filter to the requested class/group if specified.
3. Pair uploaded images with students in that order.
4. Rename images to stable lowercase slugs, e.g. `images/wind-bunny.jpg`.
5. Update each student's `image` field.

### Explicit Pairing

If the image order is unclear, show a numbered image list and ask the user to reply with pairings.

Example:

```text
圖片 1 = 吳品諭 Bunny
圖片 2 = 張恩綺 Rebecca
圖片 3 = 張奎鈞 Apple
```

Then rename, copy into `images/`, update `students.json`, validate, commit, and push.

### Image Naming Rules

- Use the student's existing `id` when available: `images/{id}.jpg` or `images/{id}.png`.
- If no `id` exists, create a lowercase ASCII slug from class and English name, e.g. `wind-bunny`.
- Preserve the original extension when reasonable; otherwise use `.jpg` for resized/compressed photos and `.png` for transparent or generated PNG artwork.
- Do not require the user to manually rename images before using this skill.

## Output Schema

`students.json` must be valid JSON:

```json
{
  "students": [
    {
      "id": "wind-bunny",
      "name": "吳品諭 Bunny",
      "group": "風班",
      "role": "Narrator",
      "story": "The Test of Courage",
      "image": "images/wind-bunny.png",
      "events": [
        "我介紹大家來到訓練中心練習戰鬥。",
        "我看到有些騎士想炫耀，也有人害怕不敢動。",
        "我聽見教練提醒大家，戰鬥是為了保護家人。",
        "我看到大家重新練習，變得更有勇氣。"
      ]
    }
  ]
}
```

Rules:

- `id`: lowercase ASCII slug when possible; include class and English name if available.
- `name`: keep Chinese name and English name together when shown in roster.
- `group`: use class as group when the user says classes are the teaching units, e.g. `風班`, `雷班`.
- `role`: preserve the drama role, e.g. `Narrator`, `Coach`, `Knight`.
- `story`: use the story title from the worksheet.
- `image`: leave `""` if the user has not provided role images yet.
- `events`: exactly 4 short Traditional Chinese first-person sentences.

## Event Writing Guidelines

Write events for elementary students and four-panel comics:

- Use first person: `我...`
- Keep each sentence short.
- Match the student's role viewpoint.
- Do not invent new plot points unless the user asks.
- Keep the same four-beat structure as the story sheet.
- Avoid abstract interpretation; write visible actions or clear thoughts.

For `The Test of Courage`, use these common role patterns:

### Narrator

```text
1. 我介紹大家來到訓練中心練習戰鬥。
2. 我看到有些騎士想炫耀，也有人害怕不敢動。
3. 我聽見教練提醒大家，戰鬥是為了保護家人。
4. 我看到大家重新練習，變得更有勇氣。
```

### Coach

```text
1. 我在訓練中心教小騎士練習戰鬥。
2. 我看到有些騎士想炫耀，也有人害怕不敢動。
3. 我提醒大家，戰鬥不是為了贏，而是為了保護家人。
4. 我看著大家重新練習，變得更勇敢。
```

### Knight

```text
1. 我在訓練中心跟著教練練習戰鬥。
2. 我看到有人犯錯，也覺得自己有點害怕。
3. 我聽見教練說，戰鬥是為了保護家人。
4. 我鼓起勇氣，和大家一起繼續練習。
```

## Workflow

1. Read the story sheet or user-provided story summary.
2. Extract story title, scene, four beats, and available roles.
3. Read the roster and role assignments.
4. Generate `students.json` according to the schema.
5. Validate JSON with Node or Python before committing:

```bash
node -e "const fs=require('fs'); JSON.parse(fs.readFileSync('students.json','utf8')); console.log('ok')"
```

6. If the user asked to publish, commit and push from the site repo:

```bash
git add students.json images
git commit -m "Update My Cat Comic student data"
git push
```

7. Verify GitHub Pages if pushed:

```bash
curl -s https://lin900212.github.io/my-cat-comic/students.json | head
```

## Publishing Safety

- Do not edit `index.html` for data-only updates unless the user asks.
- Do not include emails in `students.json` unless the user explicitly asks; the classroom website does not need them.
- Do not publish unrelated files or temporary screenshots.
- If role assignments are unknown, either ask for them or clearly mark assumptions before generating examples.
