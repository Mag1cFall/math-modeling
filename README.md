# Math Modeling Skill

数学建模竞赛三阶段工作流：建模分析 -> 代码实现 -> 论文撰写（LaTeX）。

## 目录结构

```
math-modeling-skill/
├── SKILL.md                           # 入口文件（工作流 + 产出标准）
├── references/
│   ├── roles/
│   │   ├── 建模手说明.md              # 建模分析阶段指南
│   │   ├── 编程手说明.md              # 代码实现阶段指南
│   │   └── 论文手说明.md              # 论文撰写阶段指南（含去 AI 味写作规范）
│   ├── latex-template/                # LaTeX 论文项目模板
│   ├── latex-guide.md                 # LaTeX 排版指南
│   ├── problem-decomposition.md       # 问题分解启发式指南
│   └── Outstanding Thesis/            # 优秀论文参考库（CUMCM + MCM/ICM）
├── assets/
│   ├── 算法选择决策指南.md            # 算法对比 + 反模式清单
│   └── 01-07 算法说明文档             # 算法百科参考
└── tools/
    ├── pdf/                           # PDF 读取
    ├── xlsx/                          # Excel 处理
    └── paper_search/                  # 学术文献搜索（OpenAlex）
```

## 工作流

```
建模分析 ──(分析报告 + 术语表)──> 代码实现 ──(代码 + 图表 + 结果)──> 论文撰写 ──> paper/main.pdf
```

## 产出标准

- 论文结构：每问独立（方法对比 -> 建立 -> 求解 -> 结果分析）
- 每问 >= 4 张图，每张 >= 100 字分析
- 每问先对比 >= 2 种候选方法
- 正文 <= 25 页（参考文献和附录不计入）
- 输出格式：LaTeX 项目 -> 单个 PDF

## Paper Search 配置

`paper_search` 使用 OpenAlex API（免费），需提供邮箱：

```bash
python tools/paper_search/scripts/openalex_scholar.py --query "grey prediction" --email "your@email.com"
```

## 许可证

MIT License
