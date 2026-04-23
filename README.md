<div align="center">
  <img src="https://gw.alipayobjects.com/zos/k/vl/logo.svg" width="120" />
  <h1>Kami</h1>
  <p><b>Good content deserves good paper.</b></p>
  <a href="https://github.com/tw93/kami/stargazers"><img src="https://img.shields.io/github/stars/tw93/kami?style=flat-square" alt="Stars"></a>
  <a href="https://github.com/tw93/kami/releases"><img src="https://img.shields.io/github/v/tag/tw93/kami?label=version&style=flat-square" alt="Version"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square" alt="License"></a>
  <a href="https://twitter.com/HiTw93"><img src="https://img.shields.io/badge/follow-Tw93-red?style=flat-square&logo=Twitter" alt="Twitter"></a>
</div>

## Why

Kami (紙, かみ) is a Japanese word for paper: the surface where a finished idea lands. Most AI-generated documents drift into generic corporate gray or inconsistent styling that shifts across sessions, requiring manual cleanup before they can be sent.

Kami is a document design system for the AI era: one constraint language for six formats, simple for agents to run reliably and strict enough to keep outputs coherent and ready to ship.
It supports English and Chinese directly, with Japanese via a best-effort CJK template path and a final visual QA pass before delivery.

Part of a trilogy: [Kaku](https://github.com/tw93/Kaku) (書く) writes code, [Waza](https://github.com/tw93/Waza) (技) drills habits, [Kami](https://github.com/tw93/Kami) (紙) delivers documents.

## See it

<table>
<tr>
  <td align="center" width="25%">
    <a href="assets/demos/demo-tesla.pdf"><img src="assets/demos/demo-tesla.png" alt="Tesla company one-pager"></a>
    <br><b>One-Pager</b> · 中文
    <br><sub>Tesla 公司介绍 · 单页</sub>
  </td>
  <td align="center" width="25%">
    <a href="assets/demos/demo-agent-slides.pdf"><img src="assets/demos/demo-agent-slides.png" alt="Agent keynote slides" /></a>
    <br><b>Slides</b> · English
    <br><sub>Agent keynote, 6 slides</sub>
  </td>
  <td align="center" width="25%">
    <a href="assets/demos/demo-musk-resume.pdf"><img src="assets/demos/demo-musk-resume.png" alt="Elon Musk resume"></a>
    <br><b>Resume</b> · English
    <br><sub>Founder CV, 2 pages</sub>
  </td>
  <td align="center" width="25%">
    <a href="assets/demos/demo-kaku.pdf"><img src="assets/demos/demo-kaku.png" alt="Kaku portfolio"></a>
    <br><b>Portfolio</b> · 日本語
    <br><sub>Kaku ターミナル作品集 · 7 ページ</sub>
  </td>
</tr>
</table>

## Usage

**Claude Code**

```bash
npx skills add tw93/kami -a claude-code -g -y
```

**Codex**

```bash
npx skills add tw93/kami -a codex -g -y
```

**Generic agents** (opencode, pi, and other tools that read from `~/.agents/`; `codex` is included, but use `-a codex` if you only target Codex)

```bash
npx skills add tw93/kami -a '*' -g -y
```

**Claude Desktop**

Download [kami.zip](https://cdn.jsdelivr.net/gh/tw93/kami@main/dist/kami.zip), open Customize > Skills > "+" > Create skill, and upload the ZIP directly (no need to unzip).

The ZIP is intentionally lightweight: TsangerJinKai Chinese fonts load from local checkout first, then jsDelivr CDN, not bundled inside. If rendering is off, Claude downloads them automatically on the next run.

To update: download the same ZIP URL, click "..." on the kami skill card, choose Replace, and upload the new ZIP.

The skill auto-triggers from natural requests, no slash command required. It is optimized for English and Chinese across one-pagers, long docs, formal letters, portfolios, resumes, and slides. Japanese is also supported via a best-effort CJK path, with a final visual QA pass before delivery.

Example prompts by language:

- English: `make a one-pager for my startup` / `turn this research into a long doc` / `write a formal letter` / `make a portfolio showcasing my projects` / `build me a resume` / `design a slide deck for my talk`
- 中文: `帮我做一份一页纸` / `帮我排版一份长文档` / `帮我写一封正式信件` / `帮我做一份作品集` / `帮我做一份简历` / `帮我做一套演讲幻灯片`
- 日本語: `スタートアップ向けの一枚資料を作って` / `この調査を長文レポートに整えて` / `正式な依頼文を作って` / `プロジェクト作品集を作って` / `履歴書を作って` / `登壇用スライドを作って`

To include inline charts, mention the data and chart type in your prompt. Kami embeds bar charts, line charts, and donut charts directly into the document as SVG, no external tools needed.

Chart prompts by language:

- English: `add a revenue breakdown donut chart` / `show the quarterly delivery trend as a bar chart` / `draw a line chart of user growth`
- 中文: `加一个营收结构环形图` / `用柱状图展示季度交付量` / `画一条用户增长折线`
- 日本語: `売上構成のドーナツチャートを追加して` / `四半期の出荷推移を棒グラフで示して` / `ユーザー成長の折れ線グラフを入れて`

## Design

Warm parchment canvas, ink blue as the sole accent, serif carries hierarchy, no hard shadows or flashy palettes. This is not a UI framework; it is a constraint system for printed matter. Quality documents should read as composed pages, not dashboards.

Six document types (One-Pager, Long Doc, Letter, Portfolio, Resume, Slides), with dedicated English and Chinese templates and a best-effort Japanese path. Twelve inline SVG diagram types are included: architecture, flowchart, quadrant, bar chart, line chart, donut chart, state machine, timeline, swimlane, tree, layer stack, and Venn. Kami picks the right variant based on the language you write in.

| Element | Rule |
|---|---|
| Canvas | `#f5f4ed` parchment, never pure white |
| Accent | Ink blue `#1B365D` only, no second chromatic hue |
| Neutrals | All warm-toned (yellow-brown undertone), no cool blue-grays |
| Serif | Body 400, headings 500. Avoid synthetic bold |
| Line-height | Tight titles 1.1-1.3, dense body 1.4-1.45, reading body 1.5-1.55 |
| Shadows | Ring or whisper only, no hard drop shadows |
| Tags | Solid hex backgrounds only. `rgba()` triggers a WeasyPrint double-rectangle bug |

**Fonts**: Chinese uses TsangerJinKai02 serif + Source Han Sans. Japanese currently follows the best-effort CJK path with Mincho-style fallbacks. TsangerJinKai is free for personal use, commercial use requires a license from [tsanger.cn](https://tsanger.cn). English uses Newsreader serif + Inter sans, both OFL open source.

Full spec: [design.md](references/design.md). Cheatsheet: [CHEATSHEET.md](CHEATSHEET.md).

## Background

I invest in US equities and often ask AI to generate analysis reports. The earliest drafts looked like default Google Docs: plain, inconsistent, and easy to forget. I did not want each document to shift style every time, so I kept refining the typography, palette, and spacing until the page became something I wanted to keep reading.

Later, I was invited to present my article "The Agent You Don't Know" and needed a slide deck with the same visual standard. That iteration pushed the system further: inline SVG diagrams, a unified warm palette, and tighter editorial rhythm. Over time it covered my core document formats, so it became kami, a visual language I can reuse with confidence and ship without hesitation.

## Support

- If kami helped you, [share it](https://twitter.com/intent/tweet?url=https://github.com/tw93/kami&text=Kami%20-%20A%20quiet%20design%20system%20for%20professional%20documents.) with friends or give it a star.
- Got ideas or bugs? Open an issue or PR.
- I have two cats, TangYuan and Coke. If you think kami delights your life, you can feed them <a href="https://miaoyan.app/cats.html?name=Kami" target="_blank">canned food</a>.

## License

MIT License. Feel free to use kami and contribute.
