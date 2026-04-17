# Math Modeling Skill

数学建模竞赛五阶段工作流：**前置准备 → 建模分析 → 代码实现 → 论文撰写 → 编译自查**，最终交付 LaTeX 论文项目及编译后的 PDF。

## 目录结构

```
math-modeling-skill/
├── SKILL.md                           # 入口文件（工作流 + 关键原则 + 产出标准）
├── references/
│   ├── roles/
│   │   ├── 建模手说明.md              # 建模分析阶段指南（含创新方法原则）
│   │   ├── 编程手说明.md              # 代码实现阶段指南
│   │   └── 论文手说明.md              # 论文撰写阶段指南（含去 AI 味写作规范）
│   ├── latex-template/                # LaTeX 论文项目模板（摘要独占一页、subcaption）
│   ├── latex-guide.md                 # LaTeX 排版指南（含浮动体避坑）
│   ├── problem-decomposition.md       # 问题分解启发式指南
│   └── Outstanding Thesis/            # 优秀论文参考库（CUMCM + MCM/ICM）
├── assets/
│   ├── 算法选择决策指南.md            # 算法对比 + 反模式清单 + 创新方法门槛
│   └── 01-07 算法说明文档             # 算法百科参考（按题型选读）
└── tools/
    ├── pdf/                           # PDF 读取
    ├── xlsx/                          # Excel 处理
    └── paper_search/                  # 学术文献搜索（OpenAlex，必做）
```

## 工作流

```
阶段零 前置准备（读比赛规则 + 读题 + 按题型选读算法文档）
   ↓
建模分析 ──(分析报告 + 术语表 + 文献检索)──> 代码实现 ──(代码 + 图表 + 结果)──> 论文撰写 ──> 编译自查 ──> paper/main.pdf
```

## 产出标准

- **比赛规则驱动**：页数限制 / 摘要长度 / 是否有目录 / 字号 / 参考文献格式均以比赛规则文档为准；用户未提供时默认沿用国赛惯例
- **论文结构**：每问独立（方法对比 → 建立 → 求解 → 结果分析），摘要独占一整页
- **每问 ≥ 4 张图**，每张 ≥ 100 字分析
- **每问先对比 ≥ 2 种候选方法**（允许并鼓励自创 / 组合 / 改造方法，不必硬套"高级"算法）
- **文献检索必做**：使用 `tools/paper_search` 为每问主方法搜索 ≥ 3 篇文献
- **正文页数**：默认 ≤ 25 页（参考文献和附录不计入）
- **输出格式**：LaTeX 项目 → 单个 PDF

## Paper Search 配置

`paper_search` 使用 OpenAlex API（免费），需提供邮箱：

```bash
python tools/paper_search/scripts/openalex_scholar.py --query "grey prediction" --email "your@email.com"
```

## 许可证

MIT License
