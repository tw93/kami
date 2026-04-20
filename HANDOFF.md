# kami 交接文档

给**维护** kami 的人看。**使用** kami（做文档）-> 读 `README.md` / `SKILL.md`。

## 一句话定位

Kaku（写代码）· Waza（练习惯）· **Kami（出文档）** 三部曲之一。把 Anthropic parchment 视觉语言 + 真实文档排版迭代的踩坑，封装成覆盖 6 种文档的模板 + 规范库。

**不是**：纯简历工具、Waza 的子集、Web UI framework。

## 设计铁律

全部在 `references/design.md` 开头的"设计宣言"段。任何改动之前先确认是否真的要违反那 8 条。

## 文件职责

| 文件 | 对谁 | 改动频率 |
|---|---|---|
| `SKILL.md` | Claude 运行时 | 低（只改路由表） |
| `README.md` | 外部用户 | 低（改 install / preview） |
| `HANDOFF.md` | 维护者（你） | 中 |
| `CHEATSHEET.md` | Claude + 用户 | 低 |
| `references/design.md` | Claude + 维护者 | 低（改就是改设计语言） |
| `references/writing.md` | Claude | 低 |
| `references/production.md` | Claude + 维护者 | 中（新踩坑追加） |
| `assets/templates/*.html` / `slides.py` | Claude 填内容 | 中 |
| `assets/examples/*.pdf` | README preview | 每次样式改动 |
| `scripts/build.py` | 维护者 + CI | 低 |

## 字体分发

**主字体 TsangerJinKai02-W04.ttf 是商业字体**。user-supplied only，不进 skill 分发、不进 git。

**本地开发姿势**：
- 把 `.ttf` 放到 `assets/fonts/` 下
- 所有 CN 模板和 CN demo 的 `<style>` 里都有 `@font-face { src: url("../fonts/TsangerJinKai02-W04.ttf") }`，相对路径挂载
- 有字体 -> 自动用真字体；没字体 -> fallback 链自动接手（Source Han Serif SC -> Noto Serif CJK SC -> Songti SC -> Georgia）

**打包 skill 前**：确认 `assets/fonts/*.ttf` 不在 git 里（`.gitignore` 已排除）。打 zip 或 `.skill` 时也要跳过。

**用户侧**：README / HANDOFF 里写清楚"如需主字体，把仓耳今楷 02 W04 的 .ttf 放进 `assets/fonts/`"。没字体也能跑，只是视觉降一档到 Charter / Source Han Serif。

## 关键踩坑索引（详情在 production.md Part 4）

改动前值得回看的高风险点：

1. **Tag 双层矩形**——rgba 半透明触发。必须实色 hex
2. **薄 border + 圆角双圈**——border < 1pt + border-radius 触发
3. **简历 2 页溢出**——字体 fallback / line-height / 字号动一点都会爆
4. **break-inside 在 flex 失效**——要包一层 block wrapper
5. **height: 100vh 在 @page 不准**——改用 mm 明确值
6. **SVG marker `orient="auto"` 不生效**——WeasyPrint 不旋转 marker，手绘 chevron path

## 改动的标准姿势

### 改字号 / 色 / 间距
改 `references/design.md` + 所有模板 `<style>` 里对应变量。跑 `scripts/build.py` 生成全部 PDF，对比页数没变。

### 改模板内容
**保留 CSS 不动**，只改 body。跑 `build.py` 验证页数。简历必须严格 2 页。

### 加新模板
1. Copy 最接近的现有模板做起点
2. 保持与 `design.md` 完全一致（不另创色板 / 字号）
3. 在 `SKILL.md` 路由表加一行
4. 在 `assets/examples/` 放一份生成好的 PDF
5. 若出现新独特 CSS 结构，追加到 `production.md` Part 4

## 验证流程

```bash
# 1. 生成所有
python3 scripts/build.py

# 2. CSS 扫描（rgba / 冷灰 / 1.6+ line-height）
python3 scripts/build.py --check

# 3. 字体是否真的加载了
pdffonts assets/examples/resume.pdf | head

# 4. 隐私扫描（改动后）
# 把 KEYWORDS 换成自己真实姓名 / 公司 / 电话等，避免误泄漏到模板
for kw in YOUR_NAME YOUR_COMPANY YOUR_PHONE YOUR_SCHOOL; do
  grep -rn "$kw" .
done
```

**期望页数**：one-pager 1 / letter 1 / resume 2（严格）/ long-doc 7 ± 2 / portfolio 6 ± 2。

## 打包与发布

```bash
cd /path/to/kami/parent
zip -rq kami.zip kami/ -x '*/.*'   # 忽略 .DS_Store 等
```

或用 Anthropic 官方 skill-creator 的 `package_skill`。

## 待办 / V2 候选

1. **打磨现有模板的边缘情况**（最高优先级）——portfolio 的图片占位 -> 真实 `<img>`；resume 在 Noto Serif fallback 下的 2 页严格性
2. **slides.py 补图文混排 + 数据图表版式**（两个高频场景缺口）
3. **新模板候选**：学术论文 / 证书 / 菜单 / 邀请函
4. **scripts/inspect.py** 视觉回归（对比修改前后的 PDF 渲染像素）

**不做**：打包商业字体（授权风险）；加第二种强调色（违反克制）；加暗色变体（印刷品用不上）。

---

**最后一条建议**：kami 的精神是"克制"。每次想加一个颜色、一个字重、一个阴影之前，先问"不加行不行"。多数时候答案是"行"。
