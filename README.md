# Longmao Factory

一个把「一句话需求」路由到合适执行引擎的 Skill。不写代码，只负责**先澄清、再路由、后执行**，每次交付可验证的产物。

作者：[@longmao623](https://github.com/longmao623)

## 它解决什么问题

大多数 Agent 项目把「设计网站」「写论文」「设计比赛」三件事塞进同一个工作流，结果是每次都得重新解释、产物质量随机。这个 Skill 的做法是：识别任务类型 → 问 1-3 个关键问题 → 选专用引擎 → 按量化标准验收。

## 三类任务

| 任务 | 触发 | 引擎 | 产物 |
|------|------|------|------|
| 网页搭建（简单） | 页面 ≤3、纯展示、需求明确 | 内置 `vendor/ultimate-frontend-design` | `site/` 可运行站点 |
| 网页搭建（复杂） | 页面 >3、有交互/数据流/用户流程 | Baton 分轮模式 | `site/` + `.design/` |
| 论文/文档 | 论文、报告、docx、LaTeX、数模 | 内置 Pandoc 工作流 | `.md` + `.docx` |
| 比赛设计 | 赛制、评分、循环赛、竞技场 | 内置 competition loop（Beta） | 赛制文档 + 流程图 |

判断不确定时默认走 Baton——宁可多一轮，也不要误判成简单项目做废。

## 里面有什么

```
longmao-factory/
├── SKILL.md                      # 主文件：路由规则、执行流程、验收标准
├── vendor/
│   └── ultimate-frontend-design/ # 内置网页生成引擎（第三方，Apache-2.0）
│       ├── SKILL.md
│       ├── references/           # 设计哲学、移动端模式、shadcn 规范
│       ├── scripts/              # vite / nextjs 脚手架、产物打包
│       └── templates/
├── LICENSE
└── NOTICE                        # 第三方组件归属声明
```

## 三个核心设计

**一、先搜索，再动手**

所有网页任务在写第一行代码前必须先搜索 2-3 个真实参考，并生成 `.design/DESIGN_BRIEF.md`，里面写清目标受众、三个风格关键词、参考来源，以及**必须避免的 AI 廉价感元素**（默认蓝紫渐变、glassmorphism、大圆角阴影三件套）。参考只借结构和流程，不搬视觉。

**二、量化验收，不靠感觉**

交付前必须过 0/1 门槛：`npm run build` 退出码为 0、1440px 与 375px 无重叠溢出、正文对比度 ≥ 4.5:1、单文件 ≤ 10MB。硬性项不过不能交付，软性项（如非对称布局）标注「需用户确认」。

**三、Baton 模式的单轮纪律**

复杂项目拆成多轮，每轮只做一件事。每轮结束必须更新 `history.md`，三列不可为空：完成内容、已知问题、下轮计划（没有就写「无」）。连续 3 轮无实质新增就熔断，停下来问用户。

## 安装

把整个目录放进你的 Skill 目录。以 WorkBuddy / OpenClaw 为例：

```bash
git clone https://github.com/longmao623/longmao-factory.git \
  ~/.workbuddy/skills/longmao-factory
```

其他 Agent 框架只要能读 `SKILL.md` 的 frontmatter 就能用，把路径指向 `SKILL.md` 即可。

## 可选依赖

以下不在本仓库内，不装也能跑，装了效果更好：

| 组件 | 用途 | 获取 |
|------|------|------|
| `taste-skill` | 设计禁令层，anti-slop 品质提升 | 自备（体积 36MB，第三方包） |
| `pandoc` | 论文/文档转 docx | `winget install --id JohnMacFarlane.Pandoc` |
| Playwright 或浏览器工具 | 视觉验证截图 | 各框架自选 |

没有 taste-skill 时，`SKILL.md` 里的设计增强层会降级为通用约束，路由和执行流程不受影响。

## 已知限制

- **比赛设计是 Beta**，只有骨架流程，未经实战验证，别当成品用。
- **不含 taste-skill**，追求 Awwwards 级设计感需要自己补。
- 视觉验收依赖浏览器工具，纯 CLI 环境只能做构建验证，做不了截图对比。
- 内置引擎是第三方代码，改了它不会同步上游更新。

## 许可

本仓库自有内容（`SKILL.md`、`README.md`、`NOTICE`）采用 MIT。

`vendor/ultimate-frontend-design/` 来自 [kesslerio/ultimate-frontend-design-openclaw-skill](https://github.com/kesslerio/ultimate-frontend-design-openclaw-skill) v0.1.0，Apache License 2.0，原样保留其 `LICENSE`。详见 `NOTICE`。
