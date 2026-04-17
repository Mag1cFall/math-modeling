# LaTeX 数学建模论文排版指南

本指南面向论文手，说明如何使用 `references/latex-template/` 中的 LaTeX 模板撰写数学建模竞赛论文。

---

## 环境要求

- **发行版**: TeXLive 2023+ (推荐完整安装)
- **编辑器**: TeXStudio (推荐) / VS Code + LaTeX Workshop
- **编译链**: XeLaTeX + BibTeX（中文必须用 XeLaTeX，不要用 pdfLaTeX）

### 编译命令

```bash
# 方法一：latexmk 一键编译（推荐）
latexmk -xelatex main.tex

# 方法二：手动编译
xelatex main.tex
bibtex main
xelatex main.tex
xelatex main.tex
```

TeXStudio 中将默认编译器设为 XeLaTeX，构建选项选择 `latexmk`。

---

## 项目结构

```
paper/
├── main.tex                 # 主文件（\input 各子文件）
├── abstract.tex             # 摘要 + 关键词
├── problem_restatement.tex  # 问题重述
├── problem_analysis.tex     # 问题分析
├── assumptions.tex          # 模型假设
├── symbols.tex              # 符号说明
├── question1.tex            # 问题一
├── question2.tex            # 问题二
├── ...
├── evaluation.tex           # 评价与推广
├── references.bib           # 参考文献库
├── appendix.tex             # 附录（代码 + 数据）
└── figures/                 # 图片目录（可选；见下方说明）
```

**图片目录的两种布局**（`main.tex` 已用 `\graphicspath{{../output/figures/}{./figures/}}` 兼容两者）：

1. **推荐**：代码直接输出到 `project/output/figures/`（见 SKILL.md §2.2），`paper/` 不需要自己的 `figures/`
2. **传统**：把图片复制到 `paper/figures/` 下

每个 `.tex` 子文件独立编辑，`main.tex` 通过 `\input{}` 汇总，最终编译出单个 `main.pdf`。

---

## 公式排版

### 行内公式

```latex
对于变量 $x_i$，其取值范围为 $x_i \in [0, 1]$。
```

### 独立公式（带编号）

```latex
目标函数为：
\begin{equation}
  \min Z = \sum_{i=1}^{n} c_i x_i
  \label{eq:objective}
\end{equation}
```

编号格式自动按章节生成（如式 5.1、5.2），在正文中用 `式~\eqref{eq:objective}` 引用。

### 约束条件组

```latex
\begin{equation}
  \text{s.t.} \quad
  \begin{cases}
    \sum_{i=1}^{n} a_{ij} x_i \leq b_j, & j = 1,2,\ldots,m \\
    x_i \geq 0, & i = 1,2,\ldots,n
  \end{cases}
  \label{eq:constraints}
\end{equation}
```

### 多行对齐公式

```latex
\begin{align}
  f(x) &= x^2 + 2x + 1 \label{eq:fx} \\
  g(x) &= \frac{\partial f}{\partial x} = 2x + 2 \label{eq:gx}
\end{align}
```

### 注意事项

- 每个公式一个编号，不要给不需要引用的公式编号（用 `\nonumber` 或 `equation*`）
- 公式中的文字用 `\text{}` 包裹，不要直接写中文
- 矩阵用 `pmatrix`（圆括号）或 `bmatrix`（方括号）

---

## 图片插入

### 单图

```latex
\begin{figure}[htbp]
  \centering
  \includegraphics[width=0.8\textwidth]{q1_fig1.png}
  \caption{收敛曲线对比}
  \label{fig:q1_convergence}
\end{figure}
```

### 双图并排（使用 subcaption）

```latex
\begin{figure}[htbp]
  \centering
  \begin{subfigure}[b]{0.48\textwidth}
    \centering
    \includegraphics[width=\linewidth]{q1_fig3.png}
    \caption{灵敏度分析}
    \label{fig:q1_sensitivity}
  \end{subfigure}
  \hfill
  \begin{subfigure}[b]{0.48\textwidth}
    \centering
    \includegraphics[width=\linewidth]{q1_fig4.png}
    \caption{稳定性测试}
    \label{fig:q1_stability}
  \end{subfigure}
  \caption{灵敏度分析与稳定性测试结果}
  \label{fig:q1_analysis}
\end{figure}
```

引用子图：`图~\ref{fig:q1_analysis}(a)`、`图~\ref{fig:q1_sensitivity}` 均可。

> **注意**：本模板使用 `subcaption`（现代标准），已弃用老旧的 `subfigure` 宏包。两者不要混用。

### 图片规范

- **统一使用 PNG**（300 dpi），从 `output/figures/` 引入
- 命名：`q{问题号}_fig{序号}.png`
- **默认使用 `[htbp]` 而不是 `[H]`**：允许 LaTeX 自主浮动，否则容易造成上一页大片空白（见文末「浮动体与空白页避坑」）
- 引用：`如图~\ref{fig:q1_convergence}所示`
- 图题在图下方（LaTeX 默认行为，不需要额外处理）
- 正文图 `width` 按内容宽窄自行选择（常见区间 `0.6\textwidth ~ 0.85\textwidth`），子图常取 `0.48\textwidth`

---

## 表格排版

### 三线表

```latex
\begin{table}[htbp]
  \centering
  \caption{方法对比结果}
  \label{tab:comparison}
  \begin{tabular}{lccc}
    \toprule
    \textbf{方法} & \textbf{准确率(\%)} & \textbf{时间(s)} & \textbf{收敛代数} \\
    \midrule
    遗传算法  & 95.2 & 12.3 & 85 \\
    粒子群    & 96.8 & 8.7  & 62 \\
    模拟退火  & 94.1 & 15.4 & 120 \\
    \bottomrule
  \end{tabular}
\end{table}
```

### 规范

- 必须使用 `booktabs` 的三线表（`\toprule`, `\midrule`, `\bottomrule`）
- 表题在表上方（`\caption` 在 `\begin{tabular}` 之前）
- 引用：`如表~\ref{tab:comparison}所示`
- 禁止使用竖线 `|`，中间分隔可用 `\cmidrule{col-col}`
- 大表格（> 20 行或跨页）用 `longtable`，见附录模板

---

## 参考文献

使用 BibTeX 管理。在 `references.bib` 中添加条目，在正文中用 `\cite{key}` 引用。

```latex
Dantzig\cite{dantzig1963}在1963年系统阐述了线性规划理论。
```

编译时 BibTeX 自动生成格式化的文献列表。

### 常见文献类型

```bibtex
@article{key, author={}, title={}, journal={}, year={}, volume={}, pages={}}
@book{key, author={}, title={}, publisher={}, year={}}
@inproceedings{key, author={}, title={}, booktitle={}, year={}}
```

---

## 代码附录

在 `appendix.tex` 中使用 `lstlisting` 环境插入完整代码：

```latex
\begin{lstlisting}[language=Python, caption={问题一求解代码}]
import numpy as np
# 完整代码原版粘贴
\end{lstlisting}
```

代码必须完整粘贴，不做删减。`lstlisting` 自动处理语法高亮、行号、换行。

---

## 页数控制

**所有页数限制以比赛规则文档为准**。未提供规则文档时采用国赛默认值：

| 部分 | 计入页数 | 默认限制（国赛） |
|------|---------|-----------------|
| 摘要 ~ 评价与推广 | 是 | ≤ 25 页 |
| 参考文献 | 否 | 无限制 |
| 附录（代码 + 数据）| 否 | 无限制 |

超页时精简优先级：

1. 精简冗余文字描述，保留数据和分析（先删"综上所述""值得注意的是"这类空话）
2. 把次要数据表格移到附录（用 `longtable`）
3. 把相邻两张小图合并成 `subcaption` 并排子图
4. 适当缩小正文图 `width`（从 0.85 降到 0.75）
5. **不要牺牲图片**：数模竞赛评分倾向"图多 + 分析到位"，典型每问 4-8 张，下限 3 张。宁可精简文字也别删必要的图

---

## 摘要独占一页（硬性要求）

摘要页必须只包含 **标题 + 摘要正文 + 关键词**，不允许混入目录、正文或其他章节。标准做法：

```latex
% main.tex 中的调用位置
\begin{titlepage}
  % 标题页内容
\end{titlepage}

% 摘要页：独占一页
\newpage
\input{abstract.tex}
\thispagestyle{empty}   % 摘要页不显示页码
\newpage                % 强制换页，后续内容从新页开始

% 目录（按比赛规则决定是否保留）
\tableofcontents
\newpage
```

### abstract.tex 的结构骨架

```latex
\begin{center}
  \Large\bfseries\heiti 摘\quad 要
\end{center}
\vspace{0.5em}

% ---- 摘要正文 ----
本文针对 XXX 问题，建立了 XXX 模型...（省略 400-500 字正文）

\vfill  % 把关键词推到页面底部，撑满一页

\noindent\textbf{关键词：} 关键词1；关键词2；关键词3；关键词4
```

### 撑满一页的技巧

- **核心技巧**：用 `\vfill` 把关键词推到页脚，保证摘要页在视觉上充实
- **字数不足**：增加每问"具体数值结果"的描述（评委看的就是这些数字）
- **字数溢出**：
  - 调小摘要部分行距：`\begin{spacing}{1.25} ... \end{spacing}`
  - 压缩"问题背景"描述，把字数留给方法与结果
  - 实在压不下就适度缩字号：`\small` 包裹
- **禁止**在摘要页塞入目录、标题页残留内容、导言章节
- **关键词**放在摘要正文之后，用 `\noindent\textbf{关键词：}` 起始，3-5 个关键词用中文分号或空格分隔

---

## 浮动体与空白页避坑（重要）

"一页只有一张图 + 下方大片空白"是数学建模论文最常见的排版失败。根源是：

- 用了 `[H]` 强制定位 + 图片高度较大 → 当前页剩余空间不够 → 整张图被推到下一页顶部 → 上一页留下大片空白
- 或：连续多张 `[H]` 图片堆积，把短段正文挤碎

### 推荐的浮动策略

**默认使用 `[htbp]` 而不是 `[H]`**：

| 选项 | 含义 | 场景 |
|-----|------|-----|
| `h` | here，当前位置附近 | 优先 |
| `t` | top，页面顶部 | 次选 |
| `b` | bottom，页面底部 | 次选 |
| `p` | page，单独一页（floats-only page）| 最后兜底 |
| `H` | 强制当前位置（`float` 宏包） | **仅在必须紧贴某段正文时才用** |

`[htbp]` 让 LaTeX 自主选择最佳落位，几乎不会产生"图与正文割裂+大片空白"的情况。

### `\FloatBarrier` 强制浮动体落位

当希望"问题一的图不要浮动到问题二去"时，在章节结尾加：

```latex
\usepackage{placeins}   % main.tex 的宏包引入

% 在 question1.tex 章节末尾
\FloatBarrier
```

`\FloatBarrier` 会强制所有前置未落位的浮动体在此处落位，避免跨章节漂移。推荐在 **每问的 `\section` 结束前** 加一个。

### 防止整页只有一张图

如果某张图 + caption 接近整页高度，额外加入：

1. 压缩 `width` 到 `0.75\textwidth` 甚至 `0.65\textwidth`
2. 在图前面放 2-4 行文字描述图要讲什么（"下文图X展示了..."）
3. 如果确实需要一张大图独占，用 `[p]` 让 LaTeX 把它放到专门的 float page，同时允许其他文字填充前一页：

```latex
\begin{figure}[p]
  \centering
  \includegraphics[width=\textwidth]{big_figure.png}
  \caption{...}
\end{figure}
```

### 防止章节标题孤悬页底

`\section{}` 后面只剩 1-2 行正文就换页，是"Widow/Orphan"问题。在 main.tex 的 preamble 加入：

```latex
\widowpenalty=10000
\clubpenalty=10000
```

---

## 分页与换页控制

| 命令 | 行为 | 用途 |
|-----|------|-----|
| `\newpage` | 立即换页，清空当前列 | 一般换页 |
| `\clearpage` | 换页 + 输出所有未落位浮动体 | 章节切换推荐用 |
| `\cleardoublepage` | 换到下一页且保证是奇数页 | 双面打印书稿 |
| `\pagebreak` | 允许在此换页，不强制 | 微调 |

**推荐写法**：

- 摘要 → 目录：用 `\newpage`
- 目录 → 正文：用 `\newpage`
- 章节之间：用 `\clearpage`（配合 `\FloatBarrier` 可省）
- 附录之前：用 `\clearpage`

避免滥用 `\newpage`，每多一次手动换页都可能破坏 LaTeX 自己的优化。

---

## 段落与孤行控制

| 场景 | 命令 | 用途 |
|-----|------|-----|
| 禁止段首单行留页底 | `\clubpenalty=10000` | 防止"孤儿行" |
| 禁止段末单行跑页顶 | `\widowpenalty=10000` | 防止"寡妇行" |
| 强制段落不分页 | `\nopagebreak` | 谨慎使用 |
| 适度拉长页面以容纳一行 | `\enlargethispage{\baselineskip}` | 应急微调 |

---

## 常见问题

**Q: 中文显示不出来？**
确保使用 XeLaTeX 编译，且安装了宋体、黑体等中文字体（TeXLive 完整版自带）。`ctexart` 文档类会自动处理字体回退。

**Q: 图片位置漂浮到奇怪的地方？**
改用 `[htbp]` 让 LaTeX 自主定位，配合 `\FloatBarrier` 限制浮动范围。不要全部用 `[H]`。

**Q: 公式编号格式不对？**
确保 `main.tex` 中有 `\numberwithin{equation}{section}`。

**Q: BibTeX 引用不显示？**
需要编译链 xelatex → bibtex → xelatex → xelatex。用 `latexmk -xelatex` 自动处理。

**Q: 摘要溢出第二页？**
优先压缩"问题背景"的铺垫文字，把字数留给具体方法和数值结果。实在压不下就在摘要环境里加 `\small` 或 `\begin{spacing}{1.2}`。

**Q: 图片标题被截断或字号太大？**
用 `\caption*{}`（无编号）测试实际宽度；或在 preamble 设置 `\captionsetup{font=small}`。

**Q: 代码附录行号挤到一起？**
在 `\lstset` 中调大 `numbersep`（如 `numbersep=10pt`），并把 `basicstyle` 从 `\small` 换成 `\footnotesize\ttfamily`。
