# Design System

## 设计宣言

kami 的审美可以浓缩成一句话：**暖米纸底，油墨蓝点缀，serif 承担权威，拒绝冷蓝灰与硬阴影**。

这不是一套 UI 框架，是一套印刷品的审美约束。它相信：高质量的文档读起来像文学，不像仪表盘。每条铁律的 trade-off 都是"与其多一个选择，不如少一个诱惑"。

**八条铁律**（违反之前先想清楚是否真的要违反）：

1. 页面背景 parchment `#f5f4ed`，不用纯白
2. 强调色只有油墨蓝 `#1B365D`，不引入第二种彩色
3. 所有灰色暖调（yellow-brown undertone），禁止冷蓝灰
4. 英文模板: serif 通吃标题和正文。中文模板: 标题 serif，正文 sans。UI 元素 (label, eyebrow, meta) 都用 sans
5. Serif 字重固定 500，不用 bold
6. 行距三档：紧凑标题 1.1-1.3 / 密排正文 1.4-1.45 / 阅读型 1.5-1.55。禁用 1.6+
7. Tag 背景必须实色 hex，禁止 rgba 半透明（WeasyPrint 会渲染出双层矩形）
8. 阴影用 ring 或 whisper shadow，不用硬 drop shadow
9. **禁止 italic**。所有模板和 demo 中不使用 `font-style: italic`，不需要 italic 字体文件

所有文档类型（One-Pager / Long Doc / Letter / Portfolio / Resume / Slides）都依据这份规范。这套系统来自 Anthropic 视觉语言 + 中文简历设计迭代，是两个来源的融合。

---

## 1. 色彩系统

**唯一强调色 + 纯暖调中性灰 + 零冷色**是这套设计的核心。

### 主调 · 品牌色

```css
--brand:        #1B365D;   /* Ink Blue Brand - 唯一的彩色，用于 CTA、强调、section-title 下划线 */
--brand-light:  #2D5A8A;   /* Coral Accent - 更亮的变体，偶尔用于深色底上的链接 */
```

**使用规则**：油墨蓝 `#1B365D` 全文档不超过 **5% 的面积**。超过就是堆砌，不是克制。

### 画布 · 背景色

```css
--parchment:    #f5f4ed;   /* 主页面背景 - 温暖米色，整个设计的情感基础 */
--ivory:        #faf9f5;   /* 卡片/浮起容器 - 比 parchment 更亮的暖白 */
--warm-sand:    #e8e6dc;   /* 按钮默认背景 / 明显的交互表面 */
--dark-surface: #30302e;   /* 深色主题容器 - 暖炭灰 */
--deep-dark:    #141413;   /* 深色主题页面底色 - 不是纯黑，有橄榄绿底色 */
```

**绝对禁止**：`#ffffff` 纯白作页面底 · 任何 `#f8f9fa` / `#f3f4f6` 这类冷灰底。

### 中性文字色

```css
--near-black:   #141413;   /* 主文本 - 最深但不是纯黑，有暖橄榄底色 */
--dark-warm:    #3d3d3a;   /* 次级深色文字 / 深色链接 */
--charcoal:     #4d4c48;   /* 按钮文字 / 高密度正文 */
--olive:        #5e5d59;   /* 副文本 - 描述、caption 等 */
--stone:        #87867f;   /* 三级文字 - 日期、元信息 */
--warm-silver:  #b0aea5;   /* 深色底上的浅色文字 */
```

**记忆法**：每个灰都有 **yellow-brown undertone**。如果你在 `rgb()` 里看到 R ≈ G > B（或 R > G > B 且差距很小），基本就是暖灰。冷灰是 R < G < B（偏蓝）或 R = G = B（中性）。

### 边框与分隔

```css
--border-cream: #f0eee6;   /* 最柔的边框 - 卡片默认 */
--border-warm:  #e8e6dc;   /* 明显的边框 - section 分隔 */
--border-soft:  #efede4;   /* 更淡的虚线分隔 - 列表项之间 */
--border-dark:  #30302e;   /* 深色主题下的边框 */
```

### Ring 阴影（不用传统 box-shadow）

```css
--ring-warm:    #d1cfc5;   /* 按钮 hover/focus 环 */
--ring-deep:    #c2c0b6;   /* 按下状态 */
```

### 功能色（尽量少用）

```css
--error:     #b53333;   /* 错误 - 深暖红，不刺眼 */
--focus:     #3898ec;   /* 聚焦蓝 - 唯一的冷色，只用于 input focus ring，无障碍必要 */
```

### 半透明对应实色对照表（TAG / 标签必须实色）

**原因**：WeasyPrint 渲染 rgba 半透明时 padding 区域和字形区域透明度叠加不一致，放大后产生双层矩形。详见 `production.md Part 4`。

油墨蓝 `#1B365D` 叠加在 parchment `#f5f4ed` 上的等效实色：

| 想要的 rgba 透明度 | 等效实色 hex |
|---|---|
| 0.08 | `#EEF2F7` |
| 0.14 | `#E4ECF5` |
| **0.18** | **`#E4ECF5`** ← tag 推荐默认 |
| 0.22 | `#ead3c7` |
| 0.30 | `#D6E1EE` |

---

## 2. 字体系统

### 字体栈（按优先级 fallback）

```css
/* Serif 标题（中英文） */
font-family: "TsangerJinKai02",       /* 仓耳今楷，需自备 .ttf */
             "Source Han Serif SC",    /* 思源宋体（免费，Adobe/Google 联合出品）*/
             "Noto Serif CJK SC",      /* 思源宋体的 Google 命名 */
             "Songti SC",              /* macOS 系统宋体 */
             "STSong",                 /* Windows 中文宋体 */
             Georgia, serif;

/* Sans 正文/UI（中英文） */
font-family: "Inter", "TsangerJinKai02",
             -apple-system, BlinkMacSystemFont,
             "Source Han Sans SC", "Noto Sans CJK SC",
             "PingFang SC", "Microsoft YaHei",
             Arial, sans-serif;

/* Mono 代码 */
font-family: "JetBrains Mono", "Fira Code",
             "SF Mono", Consolas, Monaco,
             "Source Han Mono", monospace;
```

### 字号层级（pt 用于 PDF，px 用于屏幕）

**印刷品（A4 PDF）用 pt**：

| 角色 | 字号 | 字重 | line-height | 用途 |
|---|---|---|---|---|
| Display | 36-48 pt | 500 | 1.10 | 封面大标题、one-pager 主标题 |
| H1 Section | 18-22 pt | 500 | 1.20 | 章节大标题 |
| H2 | 14-16 pt | 500 | 1.25 | 子章节 |
| H3 | 12-13 pt | 500 | 1.30 | 条目标题 |
| Body Lead | 11 pt | 400 | 1.55 | 导语、intro |
| Body | 9.5-10 pt | 400 | 1.55 | 正文 |
| Body Dense | 9-9.2 pt | 400 | 1.40 | 密集排版（简历、one-pager 等） |
| Caption | 8.5-9 pt | 400 | 1.45 | 说明文字、图注 |
| Label | 7.5-8 pt | 600 | 1.35 | 小标签、角标 |
| Tiny | 7 pt | 400 | 1.40 | 页脚、minor metadata |

**屏幕（网页/PPT）用 px**：乘以约 1.33 得到等效 px（9 pt ≈ 12 px，18 pt ≈ 24 px）。

### 字重规则

- **Serif**：固定 500（不用 400、不用 700）。**单一字重是设计语言的一部分**。
- **Sans 正文**：400 默认
- **Sans 标签/标题**：500 或 600
- **禁止 900 black 或 100 thin**

### 行距（line-height）三档

中文印刷品比英文网页**更紧凑**。英文网页常见的 1.6-1.75 是针对英文字母和非 fixed-width 的 body 优化的，放在中文 pt 字号的印刷品里会显得松散。

| 档位 | 值 | 用于 |
|---|---|---|
| 紧凑标题 | 1.10-1.30 | 大标题、Display、H1、H2 |
| 密排正文 | 1.40-1.45 | Body Dense（简历、one-pager、名片、索引卡） |
| 阅读型正文 | 1.50-1.55 | Body（long-doc 章节正文、letter 正文） |
| 标签 / caption | 1.30-1.40 | 小字标签、多行 metadata |

**禁用**：
- 1.60+ — 英文网页的节奏，中文印刷会显得松散
- 1.00-1.05 — 除非极致紧凑标题，否则上下文字会粘连

### 字距（letter-spacing）

- body 默认 **0** 或极轻 +0.1 pt
- 中文标题超过 20 pt 的，加 0.5-1 pt 字距
- 小字 label (<10 pt) 可以加 0.15-0.3 pt 提高可读性
- 全大写 overline 加 0.5 px（ALL CAPS 必须加字距）

---

## 3. 间距系统

### 基础单位：4 pt（或 4 px 屏幕）

| 尺度 | 值 | 用途 |
|---|---|---|
| xs | 2-3 pt | 同行内元素间距 |
| sm | 4-5 pt | tag padding、紧凑布局 |
| md | 8-10 pt | 组件内部 |
| lg | 16-20 pt | 组件之间、卡片 padding |
| xl | 24-32 pt | section 标题 margin |
| 2xl | 40-60 pt | 大 section 之间 |
| 3xl | 80-120 pt | 章节之间（长文档）|

### 页面 margin（A4）

| 文档类型 | 上 | 右 | 下 | 左 |
|---|---|---|---|---|
| Resume（紧凑）| 9 mm | 13 mm | 9 mm | 13 mm |
| One-Pager | 15 mm | 18 mm | 15 mm | 18 mm |
| Long Doc | 20 mm | 22 mm | 22 mm | 22 mm |
| Letter | 25 mm | 25 mm | 25 mm | 25 mm |
| Portfolio | 12 mm | 15 mm | 12 mm | 15 mm |

**规律**：密度越高 margin 越小，越正式（letter）margin 越大。

---

## 4. 组件样式

### Cards / Containers

```css
.card {
  background: var(--ivory);                /* 比 parchment 略浮起 */
  border: 0.5pt solid var(--border-cream);
  border-radius: 8pt;                       /* 舒适圆角 */
  padding: 16pt 20pt;
}

/* 特色卡片 */
.card-featured {
  border-radius: 16pt;                      /* 更大圆角 */
  box-shadow: 0 4pt 24pt rgba(0,0,0,0.05); /* whisper shadow */
}
```

圆角尺度：4 pt -> 6 pt -> 8 pt（默认）-> 12 pt -> 16 pt -> 24 pt -> 32 pt（hero 容器）。

### Buttons (在 PPT / 作品集 / 图表里用)

```css
/* Primary（品牌色） */
.btn-primary {
  background: var(--brand);
  color: var(--ivory);
  padding: 8pt 16pt;
  border-radius: 8pt;
  box-shadow: 0 0 0 1pt var(--brand);    /* ring shadow，不是外阴影 */
}

/* Secondary（warm sand） */
.btn-secondary {
  background: var(--warm-sand);
  color: var(--charcoal);
  padding: 8pt 16pt;
  border-radius: 8pt;
  box-shadow: 0 0 0 1pt var(--ring-warm);
}
```

### Tags / Badges

三个档位的 tag 样式，按视觉冲击力从弱到强选：

**极淡实色**（最克制、最灵动、推荐默认）：
```css
.tag {
  background: #EEF2F7;           /* rgba(201,100,66, 0.08) 等效色 */
  color: var(--brand);
  font-size: 8pt;
  font-weight: 500;
  padding: 1pt 5pt;
  border-radius: 2pt;
  letter-spacing: 0.05pt;
}
```

**标准实色**（需要稍强区分度，如多种 tag 混排时）：
```css
.tag {
  background: #E4ECF5;           /* rgba(201,100,66, 0.18) 等效色 */
  color: var(--brand);
  padding: 1pt 6pt;
  border-radius: 4pt;
}
```

**笔刷渐变**（仅在需要强化"手感"时用，慎用）：
```css
.tag {
  background: linear-gradient(to right, #D6E1EE, #E4ECF5 70%, #EEF2F7);
  color: var(--brand);
  padding: 1pt 5pt;
  border-radius: 2pt;
}
```

**设计哲学**：tint 浓度要比装饰性需求**低一档**。宁可清淡不可浓艳。实战发现"笔刷渐变"虽然技术上酷，但会抢焦点（见 production.md Part 4 #1 的实战经验）。

**绝对不用**：`background: rgba(201, 100, 66, 0.18)` —— 会触发 WeasyPrint 双层 bug。用等效实色替代。

### 列表

```css
ul, ol {
  padding-left: 16pt;
  line-height: 1.55;
}
ul li::marker {
  color: var(--brand);   /* bullet 点用品牌色 */
}
```

或者更有书卷气的**短横线代替圆点**：

```css
ul.dash { list-style: none; padding-left: 0; }
ul.dash li::before {
  content: "-  ";
  color: var(--brand);
}
```

### 引用块

```css
.quote {
  border-left: 2pt solid var(--brand);
  padding: 4pt 0 4pt 14pt;
  color: var(--olive);
  line-height: 1.55;
}
```

### 代码块

```css
.code-block {
  background: var(--ivory);
  border: 0.5pt solid var(--border-cream);
  border-radius: 6pt;
  padding: 10pt 14pt;
  font-family: "JetBrains Mono", monospace;
  font-size: 8.5pt;
  line-height: 1.5;
  color: var(--near-black);
}
```

### Section Title

```css
.section-title {
  font-family: serif;           /* 用 serif 承担所有标题 */
  font-size: 14pt;
  font-weight: 500;
  color: var(--near-black);
  margin: 24pt 0 10pt 0;
}
```

### Metric Card（数据卡）

关键指标并排显示（用于 one-pager 顶部、简历顶部、portfolio 封面）：

```css
.metrics {
  display: flex;
  gap: 24pt;
}
.metric {
  flex: 1;
  display: flex;
  align-items: baseline;
  gap: 6pt;
}
.metric-value {
  font-family: serif;
  font-size: 16pt;
  font-weight: 500;
  color: var(--brand);
  font-variant-numeric: tabular-nums;   /* 数字等宽对齐 */
}
.metric-label {
  font-size: 9pt;
  color: var(--olive);
}
```

---

## 5. 阴影与深度

**核心原则**：Claude 设计系统**不用传统硬阴影**。深度通过三种方式创造：

### 1. Ring Shadow（边框式阴影）

```css
/* 1pt 环，像 border 但更柔 */
box-shadow: 0 0 0 1pt var(--ring-warm);

/* hover 时加深 */
box-shadow: 0 0 0 1pt var(--ring-deep);
```

### 2. Whisper Shadow（极软投影）

```css
/* 几乎看不见的轻微浮起 */
box-shadow: 0 4pt 24pt rgba(0, 0, 0, 0.05);
```

### 3. 明暗交替（section 级别）

长文档里 parchment `#f5f4ed` 底 section 和 `#141413` 深色 section 交替，比任何阴影都戏剧化。

**禁止**：`box-shadow: 0 2px 8px rgba(0,0,0,0.3)` 这类传统硬阴影。

---

## 6. 打印与分页

### break-inside 保护

以下元素不允许跨页断开：

```css
.card,
.metric,
.project-item,
.quote,
.code-block,
figure,
.callout {
  break-inside: avoid;
}
```

### 强制分页

```css
.page-break { break-before: page; }
```

用于封面与正文之间、章节之间。

### 页边背景

```css
@page {
  size: A4;
  margin: 20mm 22mm;
  background: #f5f4ed;   /* 背景延伸到 margin 外，避免打印时留白边 */
}
```

---

## 7. 决策速查

遇到 "该用什么" 的时候查这张表：

| 要做什么 | 怎么做 |
|---|---|
| 大标题 | serif 500，字号根据层级，line-height 1.10-1.30 |
| 正文阅读 | sans 400，9.5-10 pt，line-height 1.55 |
| 强调一个数字 | `color: var(--brand)`，不要粗体 |
| 分隔两段内容 | 1pt 品牌色 border-bottom，或 0.5pt 暖灰虚线 |
| 引用某人的话 | 左 2pt 品牌色实线 + olive 色 |
| 展示代码 | ivory 底 + 0.5pt border + 6pt 圆角 + mono 字体 |
| 区分主次按钮 | Primary 用品牌色填充 + 白字，Secondary 用 warm-sand + charcoal |
| 在卡片列表里区分某张特殊的 | `border: 0.5pt solid var(--brand)` 或 `border-left: 3pt solid var(--brand)` |
| 章节开始 | serif 标题 + 下面一条 1pt 品牌色实线 |
| 文档封面 | 单页 Display 字号标题 + 作者/日期 right align，中间大量留白 |
| 一张数据卡 | ivory 底 + 8 pt 圆角 + serif 大数字 + sans 小标签 |

不在这张表里的情况 -> 回到原则：**serif 承担权威，sans 承担功能，暖灰承担节奏，油墨蓝承担焦点**。
