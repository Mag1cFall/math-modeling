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
└── figures/                 # 图片目录
```

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
\begin{figure}[H]
  \centering
  \includegraphics[width=0.8\textwidth]{q1_fig1.pdf}
  \caption{收敛曲线对比}
  \label{fig:q1_convergence}
\end{figure}
```

### 双图并排

```latex
\begin{figure}[H]
  \centering
  \subfigure[灵敏度分析]{
    \includegraphics[width=0.48\textwidth]{q1_fig3.pdf}
    \label{fig:q1_sensitivity}
  }
  \subfigure[稳定性测试]{
    \includegraphics[width=0.48\textwidth]{q1_fig4.pdf}
    \label{fig:q1_stability}
  }
  \caption{灵敏度分析与稳定性测试结果}
  \label{fig:q1_analysis}
\end{figure}
```

### 图片规范

- 格式优先 PDF（矢量），其次 300dpi PNG
- 命名：`q{问题号}_fig{序号}.pdf`
- `[H]` 强制图片在当前位置，避免浮动乱跑
- 引用：`如图~\ref{fig:q1_convergence}所示`
- 图题在图下方（LaTeX 默认行为，不需要额外处理）

---

## 表格排版

### 三线表

```latex
\begin{table}[H]
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
- 大表格用 `longtable` 支持跨页

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

| 部分 | 计入页数 | 限制 |
|------|---------|------|
| 摘要 ~ 评价与推广 | 是 | <= 25 页 |
| 参考文献 | 否 | 无限制 |
| 附录（代码 + 数据）| 否 | 无限制 |

如果正文超过 25 页，优先精简：
1. 减少冗余文字描述，保留数据和分析
2. 使用并排子图减少图片占用空间
3. 将次要数据表格移到附录

---

## 常见问题

**Q: 中文显示不出来？**
确保使用 XeLaTeX 编译，且安装了宋体、黑体等中文字体（TeXLive 完整版自带）。

**Q: 图片位置乱跑？**
使用 `[H]` 选项（需要 `float` 宏包），强制图片在当前位置。

**Q: 公式编号格式不对？**
确保 `main.tex` 中有 `\numberwithin{equation}{section}`。

**Q: BibTeX 引用不显示？**
需要编译 4 次：xelatex -> bibtex -> xelatex -> xelatex。用 `latexmk` 可自动处理。
