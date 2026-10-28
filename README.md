# UNNC-MacTex-Temp

UNNC 报告类文档通用 LaTeX 模板仓库（MacTeX + VS Code），用于快速启动各类 report 项目。所有个人信息与研究方向均已占位符化（`[...]`），改封面与章节占位即可用于任何报告（PhD 年审 / Research Proposal / 课程报告等）。当前内置的示例骨架为 **PhD Progression Review Report**（年审进展报告）。

## 目录结构

```
UNNC-MacTex-Temp/
└── report/                  # 报告工作目录
    ├── main.tex             # 报告主文件（Progression Review 骨架，占位待填）
    ├── bibliography.bib     # 参考文献库
    ├── Temp/
    │   ├── Report_Temp.sty  # 页面布局 / 字体 / 行距 / 目录样式等排版设置
    │   ├── IEEE_Temp.sty    # IEEE 文献样式（默认启用，biblatex + biber）
    │   └── Harvard_Temp.sty # Harvard 文献样式（备选，与 IEEE 二选一）
    └── image/               # 图片目录（含封面满幅 UNNC logo）
```

## 编译环境

- [MacTeX](https://www.tug.org/mactex/)（LaTeX 发行版，含 xelatex / latexmk / biber）
- VS Code + [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) 插件

## 编译方式

编译器默认为 **pdflatex**（英文定稿）：

- `.vscode/settings.json` 已将唯一编译 recipe 固定为 `latexmk -pdf`，保存即自动编译；
- 命令行需显式指定引擎：`latexmk -pdf main.tex`。

编译链：`pdflatex → biber → pdflatex × 2`（由 latexmk 自动完成）

**命令行：**

```bash
cd report
latexmk -pdf main.tex
```

**VS Code：** 打开 `report/main.tex`，保存（⌘S）即自动编译；`⌘⌥V` 打开 PDF 预览，支持正反向跳转（SyncTeX）。

**切换 XeLaTeX（备用，正文需要输出中文时）：**

1. `.vscode/settings.json`：注释掉 pdflatex 的 tools / recipes 两段，取消文件底部 xelatex 注释段；
2. `report/main.tex` 顶部 magic comment 改为 `% !TeX program = xelatex`。

`Temp/Report_Temp.sty` 已做引擎自适应（XeLaTeX 下才加载 xeCJK），切换无需改 sty。源码中的中文注释两种引擎下都不受影响。

## 开新 Report

1. 从本模板仓库新建仓库（或复制 `report/` 目录）；
2. 修改 `main.tex` 封面的系所、报告类型（如 PhD Progression Review Report / Research Proposal）、学年、标题与信息表；
3. 按报告类型替换正文章节占位内容（方括号 `[...]` 处），甘特图 / 时间线表格可原样复用；
4. 在 `bibliography.bib` 中添加参考文献，正文用引用命令（见下）。

## 写作约定

- 正文与表格统一放在 `main.tex`，图片放 `image/`
- 源码中可直接写中文注释（两种引擎均支持）；正文要**输出**中文则需切换 XeLaTeX（见上文编译方式）
- **文献样式二选一**：`main.tex` 开头 `\usepackage{Temp/IEEE_Temp}`（IEEE，默认）与 `\usepackage{Temp/Harvard_Temp}`（Harvard）对调注释即可，两者不可同时加载
- 引用命令示例（条目键为 `examplebib`）：
  - IEEE：`A citation command in IEEE.\cite{examplebib}`
  - Harvard：括号引用 `\parencite{examplebib}`；行文引用 `As \textcite{examplebib} said \dots`；自动适配 `\autocite{examplebib}`
- 插图示例：

```latex
\begin{figure}[H]
    \centering
    \includegraphics[width=0.8\textwidth]{image/xxx.png}
    \caption{Example caption for example picture}
    \label{fig:xxx}
\end{figure}
```

- 甘特图用 `pgfgantt` 绘制（横向长图放在 `landscape` 环境内）；已完成的任务条可加 `progress=100` 选项标注进度
- 编译中间文件（`.aux`、`.log`、`.bbl`、`.bcf` 等）及 `report/main.pdf` 已被 `.gitignore` 忽略，无需手动清理
