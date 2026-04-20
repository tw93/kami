# Diagrams

kami 的绘图能力。**不重新造 13 种图**，只做 3 种最通用的、能嵌进 long-doc / portfolio / slides 里增强论述的图：架构、流程、象限。皮肤 100% 沿用 kami 设计语言（parchment + 油墨蓝 + 暖灰），不引入第二种设计系统。

所有图都是**自包含的 HTML + inline SVG**。没有 Mermaid、没有 JS、没有构建步骤。要么作为独立页面浏览，要么把 `<svg>...</svg>` 块整段复制进 long-doc 的 `<figure>` 里。

---

## 1. 选择表

| 你要表达的是… | 选这个 | 模板 |
|---|---|---|
| 系统组件 + 它们之间的连接 | **架构图** | `assets/diagrams/architecture.html` |
| 决策分支 · "如果 A 则 B，否则 C" | **流程图** | `assets/diagrams/flowchart.html` |
| 两个维度上的定位 / 优先级 | **象限图** | `assets/diagrams/quadrant.html` |

不在这三种里的情况：
- **时序 / 阶段**：用 long-doc 自带的 timeline 组件（resume.html / one-pager.html 里都有）。不新造图。
- **对比两件事**：用表格。三列就能说清的事不要画图。
- **hierarchy / 缩进列表**：markdown 嵌套 ul 就够。
- **一个方框一个标签**：删掉方框，直接写一句话。

### 绘图前的质问

> 如果把这张图换成一段好写的话，读者学到的会少吗？

答"不会"就别画。图是给"层次、方向、量级"加信号的，不是给文字加装饰的。

---

## 2. 克制密度

**目标密度 4/10**。够表达完整系统，但读者不需要图例指南就能看懂。

- 节点数 > 9 -> 这不是一张图，是两张
- 两个永远一起出现的节点 -> 合并成一个
- 有线但层级从位置就能读出来 -> 去掉这条线
- 5 个节点都是油墨蓝 -> 你没想清楚什么是焦点

**焦点规则**：一张图 1-2 个焦点（`#1B365D` + `#EEF2F7` 填充）。其他全走中性色。焦点的意义来自对比，不是数量。

---

## 3. 嵌入 long-doc / portfolio 的姿势

### 独立浏览

直接打开 `assets/diagrams/architecture.html`（或 flowchart / quadrant）。每份都是完整 HTML，包含标题 + SVG + caption。

### 嵌入到 kami 文档里

从模板 HTML 里**只抠 `<svg>...</svg>` 块**（不要带 frame / h1 / eyebrow），丢进 long-doc 的 `<figure>` 里：

```html
<figure>
  <svg viewBox="0 0 960 460" xmlns="http://www.w3.org/2000/svg">
    <!-- 从 architecture.html 里复制的 svg 内容 -->
  </svg>
  <figcaption>图 1 · {{简短描述，编辑风}}</figcaption>
</figure>
```

long-doc.html 的 `<style>` 已经定义好 figure / figcaption 样式，不需要额外 CSS。

### 改动节点 / 文字

直接改 SVG 里的 `<text>` 和 `<rect>` 坐标。规矩：

- **所有坐标、宽度、间距都能被 4 整除**。这一条是防 AI-slop 的底线，违反之后图就会开始像"差不多就行"。
- 节点宽度：128 / 144 / 160（三档够用，别再加）。小图（viewBox 宽 < 360）允许缩到 2 档，但仍限定 2 档，不要每个节点量体裁衣。
- 节点高度：32（pill）/ 64（标准）
- 字号：7（小标签 mono）/ 9（sublabel mono）/ 12（name sans）
- **箭头端点落在节点边缘**：起点 `(box.x + box.w, box.y + box.h/2)`、终点 `(box.x, box.y + box.h/2)`，不能"大概靠近"。悬空 10px 人眼就看得出来。
- **SVG 顶部文字留白**：SVG 的 `<text y="…">` 是 baseline 位置。y 必须 ≥ font-size × 1.2，否则字母顶部伸出 viewBox 被裁（典型是 "TOOLS" 变 "TOULS"）。要么放宽 viewBox 顶部、要么把 y 加到安全区。
- **环形图的弧线控制点**：四节点闭环，每条弧用 Q-curve，控制点落在**两节点切线轴的外侧交点**，不是节点的角。例如 PLAN（上）→ ACT（右）这段弧：起点是 PLAN 右边缘中点、终点是 ACT 顶边缘中点，控制点是 `(ACT.x + ACT.w/2, PLAN.y + PLAN.h/2)`。这样出发切线是纯水平、到达切线是纯垂直，弧形读起来像标准四分之一圆。控制点落在节点角上会把弧挤扁。
- **闭环图加一层 dashed 圆框**：四条有向弧单独存在时，读者要在脑里自己拼成环。加一个 dashed 圆圈（圆心在视觉中心，半径略大于中心到节点内缘的距离），瞬间说清"这是一个闭环"。圆画在节点下面，节点实色填充遮住圆穿过节点那一段，只在节点之间可见。
- **chevron 箭头而非实心三角**：`<path d="M2 1 L8 5 L2 9" fill="none" stroke=... stroke-width="1.5" stroke-linecap="round"/>`。实心三角读起来像技术 UI，两笔 open chevron 是编辑感。kami 默认用 chevron。**注意 WeasyPrint 不支持 `<marker orient="auto">`**，marker 方向全部固定为 0°（朝右）。解法是不用 marker，每个箭头端点手动画一条 chevron `<path>`，方向写死（详见 production.md #15）。

### 颜色 token 映射

三种图共用的 token 角色，直接对应 kami 设计系统：

| SVG 角色 | kami token | 值 |
|---|---|---|
| 画布 | `--parchment` | `#f5f4ed` |
| 标准节点填充 | （白）| `#ffffff` |
| 标准节点描边 | `--near-black` | `#141413` |
| Store 节点填充 | near-black 5% | `rgba(20,20,19,0.05)` |
| Store 节点描边 | `--olive` | `#5e5d59` |
| Cloud 节点填充 | near-black 3% | `rgba(20,20,19,0.03)` |
| Cloud 节点描边 | near-black 30% | `rgba(20,20,19,0.30)` |
| External 节点填充 | olive 8% | `rgba(94,93,89,0.08)` |
| External 节点描边 | `--stone` | `#87867f` |
| **焦点填充** | `--brand-tint` | `#EEF2F7` |
| **焦点描边** | `--brand` | `#1B365D` |
| 标准箭头 | `--olive` | `#5e5d59` |
| 焦点箭头 | `--brand` | `#1B365D` |
| 文本主色 | `--near-black` | `#141413` |
| 文本辅助色 | `--olive` | `#5e5d59` |
| 文本三级色 / 小 mono 标签 | `--stone` | `#87867f` |

别加第四种状态（比如 "warning 橙" 或 "success 绿"）。kami 只有一种强调色。

---

## 4. AI slop 反模式（看到这些就知道是 AI 默认输出）

写图 / review 图的时候扫一遍：

| 反模式 | 为什么失败 |
|---|---|
| 深色底 + 青紫色辉光 | "科技感"的廉价符号，没有设计决策 |
| 所有节点一样大小 | 消除层级 |
| JetBrains Mono 当通用"开发"字体 | Mono 只给技术内容（端口、URL、字段类型）。名字用 sans |
| 图例浮在图内部 | 和节点冲撞 |
| 箭头文字没有遮罩矩形 | 文字被线穿过去 |
| 箭头上竖排文字 | 读不懂 |
| 默认 3 个等宽总结卡片 | 模板感，宽度需要有变化 |
| 任何元素加 shadow | kami 只用 ring / whisper shadow |
| `rounded-2xl` / `border-radius: 16px+` 给节点 | 最大 6-10 px 圆角，否则开始像 App Store UI |
| 油墨蓝撒满每个"重要"节点 | 焦点规则是 1-2 个，不是信号系统 |
| 表情符号 🚀 📊 💡 做图标 | 灾难 |
| 渐变背景 | kami 禁用 |
| 焦点与标题论点不一致 | 标题说 "Simple **core**"，图里却把 ACT 染成油墨蓝，两个 focal 打架。focal 的颜色必须和 caption 里用 `<span class="hl">` 强调的那个词对应 |
| 循环图里既画虚线环、又画四段有向弧 | 同一条回路被画两遍，读者以为有两套流 |
| SVG 文字顶部被 viewBox 裁切 | text 的 y 是 baseline，字母顶部伸到 y 负值区域。给顶部留 font-size × 1.2 的 padding，或改 viewBox |
| 箭头起点/终点与节点边缘有 5-10px 缝隙 | 看起来像"箭头悬在半空"。endpoints 要锚到 `box.x / box.x+w / box.y / box.y+h` 的精确值 |
| 同一张图里节点大小各自量体裁衣 | 四个步骤的宽度 60/76/80/100 给人"手工凑出来"的感觉。小图 2 档、大图 3 档，够了 |
| 参考外部图表时把每类节点着一个 accent 色（紫/琥珀/绿/红） | kami 只有一种强调色。移植外部图时，把 focal 迁移到 caption 里 `<span class="hl">` 强调的那个词上，把颜色集中在那一个元素，不要铺给每个节点 |
| 环形图所有节点都是单词、中心什么都没有 | 四个框轮流转，读者没有锚。要么节点带副标题，要么环心放一句话（"20 LOC"、exit condition 等），二选一 |

---

## 5. 常见组合

### 技术白皮书
- 架构图（系统全景）+ 时序组件（long-doc 自带的 timeline）
- 每章最多 1 张架构图。超过 1 张说明这一章讲了两个东西，应该拆章

### 作品集项目页
- 象限图（竞争定位）或架构图（你负责的那一层）
- **不要**每个项目都画图。只在这张图能说清而文字说不清的时候才画

### 一页纸方案
- 象限图（优先级） 或 流程图（决策路径）
- 只能有 1 张。如果想放 2 张，砍掉文字凑图的冲动，选最关键的那张

### 简历
- **不画图**。简历空间比图值钱。极少数例外：展示系统架构能力时，附一个 URL 指到 portfolio

### Slides
- 每页最多 1 张图。图占屏幕主体，文字是 caption，不是另外一栏

---

## 6. 构建 / 预览

```bash
python3 scripts/build.py diagram-architecture
python3 scripts/build.py diagram-flowchart
python3 scripts/build.py diagram-quadrant

# 或全部
python3 scripts/build.py
```

浏览器里打开 `assets/diagrams/*.html` 也能直接看。

---

## 7. 参考

这套能力的灵感来自 Cathryn Lavery 的 [diagram-design](https://github.com/cathrynlavery/diagram-design)（Claude Code skill，13 种编辑风 diagram）。kami 只吸收了它的**做法**（inline SVG、语义 token、复杂度预算、反 AI-slop 表），没有整体移植——13 种会把 kami 撑开，3 种够用于所有 kami 文档场景。
