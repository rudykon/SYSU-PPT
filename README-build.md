# 编译说明

使用 XeLaTeX + latexmk 编译，提供英文版 `main.tex` / `main.pdf` 和
中文版 `main-zh.tex` / `main-zh.pdf`（均为 17 页，含 Beamer 动画分页）。

在本目录执行：

```bash
latexmk -interaction=nonstopmode -halt-on-error main.tex
```

编译中文版：

```bash
latexmk -interaction=nonstopmode -halt-on-error main-zh.tex
```

`.latexmkrc` 已设置 XeLaTeX，并使用项目内 `.texmf` 中的补充依赖。

中文版翻译了封面、目录、示例正文、图注、代码注释、致谢及时间轴节点，
保留模板原有的作者姓名和邮箱，可在 `main-zh.tex` 的导言区替换。
中文字体优先使用 Noto CJK，未安装时回退到 TeX Live 自带的 Fandol 字体。
中文版与英文版共用主题、校徽和时间轴样式。

## 页脚时间轴

已新增 `beamerouterthemesysutimeline.sty`，并在两个版本中启用。
原有 `beamerthemeVerona.sty` 和校徽图片无需修改。

当前示例只有一个大章节，因此按当前章节内的四个小节显示节点：

```latex
\usetheme[purple,colorblocks]{Verona}
\useoutertheme[subsections]{sysutimeline}
```

如果演示文稿包含多个大章节，改用：

```latex
\useoutertheme{sysutimeline}
```

节点自动读取 `\section` / `\subsection`，无需手动填写位置或页码。
绿色实心表示已讲到的节点，绿色双圈和加粗标题表示当前节点，灰色空心表示后续节点。
点击节点或名称可跳转到对应章节/小节的起始页。
动画分页保持当前节点不变。
时间轴替换原来作者、单位和演示标题所在的页脚区域，右侧保留页码；页脚不再显示这些文字。
封面、致谢等 `[plain]` 页面自动隐藏页脚。
没有可用章节节点时显示按逻辑幻灯片页码计算的进度条。

建议每条时间轴使用 3–6 个节点，并用简短名称避免换行过多。
长标题可以指定导航用的短标题，例如：

```latex
\section[研究方法]{研究方法与技术路线}
\subsection[实验结果]{实验结果与对比分析}
```

新增或调整章节后重新运行 `latexmk`，它会自动完成导航需要的多次编译。
如需关闭时间轴，注释掉 `\useoutertheme...{sysutimeline}` 即可恢复原页脚。

## 编译状态

启用时间轴后，英文版没有横向/纵向溢出警告。
英文版仍有模板原有的非致命警告：`inputenc` 在 XeLaTeX 下忽略、默认中文字体提示、
未定义的 `zhli` 字体族。
英文版没有缺字警告。中文版显式设置中文字体，不加载 `inputenc`；
中文版已通过 17 页编译与排版检查，无字体、缺字或横向/纵向溢出警告。
编译后可在本地查看 `main.log` / `main-zh.log`；这些日志不上传到仓库。
需要保存完整终端输出时，可在编译命令后添加 `> build.log 2>&1`
（中文版可使用 `> build-zh.log 2>&1`）。
