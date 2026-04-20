# Kami Design System · Web Page

一页可编辑的设计系统参考页。部署到 GitHub Pages 或直接本地打开浏览。

## 文件结构

```
site/
├── index.html      # 页面主体（可直接编辑 HTML 内容）
├── styles.css      # 所有样式（可直接调色 / 改字号）
└── README.md       # 本文件
```

字体文件复用仓库自带的：`../assets/fonts/TsangerJinKai02-W04.ttf`（相对路径已在 `styles.css` 配好）。

## 本地预览

直接用浏览器打开 `site/index.html` 即可。

或者用任意静态服务器：
```bash
cd site && python3 -m http.server 8000
# 访问 http://localhost:8000
```

## 部署到 GitHub Pages

1. 把 `site/` 整个目录提交到仓库
2. 仓库 **Settings → Pages**
3. Source 选 **Deploy from a branch**
4. Branch 选 `main` · Folder 选 `/site`
5. Save，几分钟后访问 `https://<you>.github.io/kami/`

> ⚠️ 因为 `styles.css` 里引用了 `../assets/fonts/TsangerJinKai02-W04.ttf`，`site/` 必须和 `assets/` 位于同一层级。你仓库本身的结构刚好满足。

## 导出 PDF

浏览器里按 **Cmd/Ctrl + P**：
- 纸张：A4
- 边距：默认
- ✅ 勾选"背景图形"（否则米色底会丢失）
- 保存为 PDF

`@page` 规则已经把页边背景延伸为 parchment，分页保护也已内置（卡片、引用、指标不会跨页断开）。

## 二次编辑指南

**改色** → 打开 `styles.css` 顶部 `:root { ... }`，所有 token 在这里集中定义。

**改字号** → 搜 `.type-sample.display` / `.hero h1` 等具名样式。

**改内容** → 改 `index.html`，样式完全由 class 驱动，改文案不影响布局。

**加章节** → 复制一个 `<section>` 块粘到下方，按 `.section-head` → `.section-num` / `.section-title` / `.section-lede` 的结构填。

**改字体** → 若要换 serif 主字，改 `styles.css` 里 `--serif` 变量的回退链顺序即可。
