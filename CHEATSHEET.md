# kami · Cheatsheet

一页纸速查。填模板 / 调细节前扫一眼。完整规范在 `references/design.md`。

## 八条铁律

1. 页面背景 `#f5f4ed`（parchment），不用纯白
2. 强调色只有油墨蓝 `#1B365D`
3. 所有灰**暖调**（yellow-brown undertone），禁冷蓝灰
4. 标题 serif，正文/UI sans
5. Serif 字重固定 500，不用 bold
6. 行距：标题 1.1-1.3 / 密排 1.4-1.45 / 阅读 1.5-1.55。禁 1.6+
7. Tag 背景实色 hex，禁 rgba（WeasyPrint 双层矩形 bug）
8. 阴影用 ring 或 whisper，不用硬 drop shadow

## 色板

| 角色 | Hex | 用途 |
|---|---|---|
| Parchment | `#f5f4ed` | 页面底 |
| Ivory | `#faf9f5` | 卡片 / 浮起容器 |
| Warm Sand | `#e8e6dc` | 按钮背景 / 交互面 |
| Dark Surface | `#30302e` | 深色容器 |
| Deep Dark | `#141413` | 深色页面底 |
| **Brand** | **`#1B365D`** | **强调 · CTA · 标题下划线（全文 ≤ 5%）** |
| Brand Coral | `#2D5A8A` | 深底上的链接 |
| Near Black | `#141413` | 主文字 |
| Dark Warm | `#3d3d3a` | 次级深色 / 链接 |
| Charcoal | `#4d4c48` | 按钮文字 / 高密度正文 |
| Olive | `#5e5d59` | 副文本 · 描述 |
| Stone | `#87867f` | 三级文字 · 元信息 |
| Warm Silver | `#b0aea5` | 深底上的浅色文字 |
| Border Cream | `#f0eee6` | 卡片默认边 |
| Border Warm | `#e8e6dc` | section 分隔 |
| Ring Warm | `#d1cfc5` | 按钮 hover / focus 环 |

**rgba -> 实色对照**（底 parchment + 前景油墨蓝）：

| 透明度 | 实色 |
|---|---|
| 0.08 | `#EEF2F7` |
| 0.14 | `#E4ECF5` |
| **0.18** | **`#E4ECF5`** ← 默认 tag |
| 0.22 | `#ead3c7` |
| 0.30 | `#D6E1EE` |

## 字号（印刷品 pt）

| 角色 | 字号 | 字重 | line-height |
|---|---|---|---|
| Display | 36-48 | 500 | 1.10 |
| H1 | 18-22 | 500 | 1.20 |
| H2 | 14-16 | 500 | 1.25 |
| H3 | 12-13 | 500 | 1.30 |
| Body Lead | 11 | 400 | 1.55 |
| Body | 9.5-10 | 400 | 1.55 |
| Body Dense | 9-9.2 | 400 | 1.40 |
| Caption | 8.5-9 | 400 | 1.45 |
| Label | 7.5-8 | 600 | 1.35 |
| Tiny | 7 | 400 | 1.40 |

屏幕（px）≈ pt × 1.33。

## 间距（4pt 基）

| 级 | 值 | 用途 |
|---|---|---|
| xs | 2-3 pt | 同行内 |
| sm | 4-5 pt | tag padding |
| md | 8-10 pt | 组件内部 |
| lg | 16-20 pt | 组件之间 |
| xl | 24-32 pt | section 标题 margin |
| 2xl | 40-60 pt | 大 section 之间 |
| 3xl | 80-120 pt | 长文档章节之间 |

**页面 margin（A4）**

| 文档 | 上右下左 |
|---|---|
| Resume | 9 mm 13 mm 9 mm 13 mm |
| One-Pager | 15 / 18 / 15 / 18 mm |
| Long Doc | 20 / 22 / 22 / 22 mm |
| Letter | 25 mm 全周 |
| Portfolio | 12 / 15 / 12 / 15 mm |

## 圆角尺度

`4 pt -> 6 pt -> 8 pt（默认）-> 12 pt -> 16 pt -> 24 pt -> 32 pt（hero）`

## 常用 CSS 片段

### Card

```css
.card {
  background: var(--ivory);
  border: 0.5pt solid var(--border-cream);
  border-radius: 8pt;
  padding: 16pt 20pt;
}
```

### Tag（默认极淡实色）

```css
.tag {
  background: #EEF2F7;          /* 0.08 等效 */
  color: var(--brand);
  font-size: 8pt; font-weight: 500;
  padding: 1pt 5pt;
  border-radius: 2pt;
  letter-spacing: 0.05pt;
}
```

### Section Title（品牌色下划线是签名式样）

```css
.section-title {
  font-family: serif;
  font-size: 14pt; font-weight: 500;
  color: var(--near-black);
  margin: 24pt 0 10pt 0;
}
```

### Metric（数据卡）

```css
.metric { display: flex; align-items: baseline; gap: 6pt; }
.metric-value {
  font-family: serif; font-size: 16pt; font-weight: 500;
  color: var(--brand);
  font-variant-numeric: tabular-nums;
}
.metric-label { font-size: 9pt; color: var(--olive); }
```

### Quote

```css
.quote {
  border-left: 2pt solid var(--brand);
  padding: 4pt 0 4pt 14pt;
  color: var(--olive);
  line-height: 1.55;
}
```

## 决策速查

| 想做 | 怎么做 |
|---|---|
| 大标题 | serif 500，line-height 1.10-1.30 |
| 正文阅读 | sans 400，9.5-10pt，1.55 |
| 强调数字 | `color: var(--brand)`，不加粗 |
| 分两段 | 1pt 品牌色 border-bottom，或 0.5pt 暖灰虚线 |
| 引用 | 左 2pt 品牌实线 + olive 色 |
| 代码 | ivory 底 + 0.5pt border + 6pt 圆角 + mono |
| 主按钮 | 品牌色填充 + ivory 字 |
| 次按钮 | warm-sand 底 + charcoal 字 |
| 章节开始 | serif 标题 + 下方 1pt 品牌色实线 |
| 封面 | 单页 Display 标题 + 右对齐作者/日期 + 大量留白 |

不在表里 -> 回原则：**serif 承担权威，sans 承担功能，暖灰承担节奏，油墨蓝承担焦点**。
