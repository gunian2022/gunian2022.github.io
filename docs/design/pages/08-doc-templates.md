# 开发文档模板台 · pages/doc-templates.html

> 按 docs 目录规划取可执行骨架：design 定「有什么」、spec 定「做成什么样」、tasks 定「怎么拆」，选文件 → 复制 → 直接贴给 AI Agent。

---

## 1. 元信息

| 字段 | 内容 |
| --- | --- |
| 页面文件 | [`pages/doc-templates.html`](../../../pages/doc-templates.html) |
| 线上路径 | `/pages/doc-templates.html` |
| 页面类型 | 文档工程模板库（目录树 + 只读预览 + 复制 / ZIP） |
| `<title>` | 开发文档模板台 · 工作台 |
| 样式文件 | [`css/work-hub.css`](../../../css/work-hub.css)（底座）+ [`css/doc-templates.css`](../../../css/doc-templates.css)（`dt-` 前缀） |
| 脚本文件 | [`script/studio.js`](../../../script/studio.js)（复制 / Toast）、[`js/project-doc-templates.js`](../../../js/project-doc-templates.js)（模板数据与自检）、[`js/doc-templates.js`](../../../js/doc-templates.js)（渲染、形态切换、导出、存储）、[`js/gate.js`](../../../js/gate.js)（密码门） |
| 依赖资源 | 无外部依赖、无构建；ZIP 由前端 Blob 生成 |
| 数据来源 | 模板内置（`kit.version = 3`）；`localStorage` 键 `work-hub.doc-templates.v2` 只存「形态 / 当前模板 / 项目名称 / 技术栈」；首次加载从 v1 迁移项目名称与技术栈后删除旧键 |
| 访问控制 | 密码门（`data-pw-field="work-hub-PW"`，前端校验，弱隔离） |
| 文档更新 | 2026-11-03 |

## 2. 页面定位

- **给谁看**：新项目开工前的自己，以及需要把文档规范一次性交给 AI Agent 的人。
- **核心任务**：按固定的 docs 目录给出**可直接照做的规范骨架**——design 写清每个字段是什么、为什么有、什么场景用；spec 把颜色、字号、按钮、按钮下拉框、弹窗、滚动条钉成带色值和像素的标准，并给出可判定的 AC 验收；tasks 拆成每页 12 条能勾选的小任务。复制或下载后交给 Agent 即可开工。
- **产品特征**：先讲三份文档分工，再画目录树；右侧只读预览并标出骨架行数与体积；页面 >5 时 14 个位置（spec 下多一份全局设计规范），≤5 时 11 个位置（全局规范并入 spec.md）；支持整套复制与 ZIP 落盘。
- **不做什么**：不提供在线编辑与正文草稿，不调用在线 AI，不生成模板以外的项目内容。

## 3. 页面结构

| # | 区块 | 锚点 / 标识 | 主要内容 | 源码位置 |
| --- | --- | --- | --- | --- |
| 1 | 密码门 | `#gate` | 访问密码，解锁前隐藏页头与主内容 | 第 13–19 行 |
| 2 | 页头 / 导航 | `.hub-header` `.hub-nav` | 品牌、四页互链、返回主页 | 第 20–22 行 |
| 3 | 首屏 | `.hub-hero` | 标题、说明、5 个 chip（当前形态 / 位置数 / 含全局设计规范 / 双端同一套令牌 / 只读） | 第 24–29 行 |
| 4 | 三份分工 | `.dt-role` `.dt-role-card` | 三张并列卡：DESIGN 有什么 / SPEC 做成什么样 / TASKS 怎么拆，各带 3 条要点 | 第 31–71 行 |
| 5 | 目录结构 | `#dtTree` `#dtRules` `.dt-naming` `#dtCopyTree` | 形态切换、CSS 目录树、10 条规则、命名速查、复制按钮 | 第 73–101 行 |
| 6 | 模板工作台 | `.dt-fields` `#dtCopyAll` `#dtZip` `#dtNavToggle` `#dtNav` `#dtDetail` | 两个资料输入、整套复制 / ZIP、文件导航、骨架预览（含行数与体积）与单份复制 | 第 103–135 行 |
| 7 | 怎么用 | `.dt-steps` | 开工 → 设计 → 规范 → 拆任务 → 开发验收 5 步 | 第 137–152 行 |
| 8 | 页脚说明 | `.dt-foot` | 本地存储范围与占位符说明 | 第 154 行 |

## 4. 布局

- **容器**：`.hub-wrap` `width: min(1140px, calc(100% - 48px))`。
- **三份分工**：`.dt-role` 三栏 `repeat(3, minmax(0, 1fr))`，间距 14px；卡片等高（grid 自动拉伸），中间 SPEC 卡用 `.dt-role-card--focus` 提亮描边与底色，形成视觉焦点。
- **目录结构**：`.dt-map-grid` 两栏 `minmax(0, 1.02fr) / minmax(260px, .98fr)`；左列树卡片按内容宽度收缩（`justify-self:start`），最大高 470px，超出内部滚动；右列为规则列表 + 命名速查。
- **模板工作台**：`.dt-work` 两栏 `276px + minmax(0, 1fr)`；左栏 `.dt-nav` 在桌面 `position: sticky; top: 18px`，最大高 `calc(100vh - 36px)`，内部滚动。
- **详情**：`.dt-detail` 左侧 1px 分隔线 + 22px 内边距；元信息三列（何时写 / 谁引用 / 骨架规模）；骨架区最大高 `min(68vh, 760px)`，超出内部滚动。
- **资料输入**：`.dt-fields` 两列，每列 `minmax(150px, 220px)`；标签在上、输入在下。
- **断点**：≤760px 目录两栏改单栏、树卡片撑满；≤900px 三份分工单列、工作台单栏、导航变折叠选择器；≤620px 形态按钮撑满、详情元信息与流程单列。

## 5. 视觉样式

### 5.1 颜色

| 用途 | 变量 / 类名 | 色值 |
| --- | --- | --- |
| 页面底色 | `--bg` | `#101821` |
| 面板 | `--panel` | `#182530` |
| 正文 | `--text` | `#edf3f4` |
| 次要文字 | `--muted` | `#a0b2ba` |
| 分隔线 | `--line` | `#c5dedc26` |
| 薄荷主色 | `--mint` | `#80e0c1` |
| 荧光黄强调 | `--lime` | `#d4f178` |
| 树卡片底色 | `.dt-tree` | `#0d1720` |
| 树连接线 | `.dt-fs li::before/::after` | `#c5dedc33` |
| 分工卡底色 | `.dt-role-card` | `#0d1720aa` |
| 分工卡焦点底色 | `.dt-role-card--focus` | `#80e0c10a` |
| 分工卡焦点描边 | `.dt-role-card--focus` | `#80e0c155` |
| 分工卡列表点 | `.dt-role-list li::before` | `--mint` / .7 |
| 未填占位提示 | `.dt-hint` | `#ffbd78` |
| 状态错误 | `.dt-status-error` | `#ffbd78` |
| 页脚文字 | `.dt-foot` | `#72858b` |

### 5.2 字体

| 角色 | 字号 / 字重 | 说明 |
| --- | --- | --- |
| 首屏 `h1` | `clamp(30px, 4.5vw, 48px)` / 700 | 字距 -.04em |
| 面板 `h2` | 20px / 650 | |
| 分工卡小标 `.dt-role-kicker` | 10.5px 等宽 / 600 | 字距 .12em，全大写，薄荷色 |
| 分工卡标题 | 15px / 600 | 行高 1.45 |
| 分工卡正文 | 12.5px / 400 | 行高 1.7 |
| 分工卡要点 | 11.5px / 400 | 行高 1.65，前 5px 圆点 |
| 详情标题 `.dt-title` | 17px / 600 | |
| 文件名 `.dt-doc-path` | 11.5px 等宽 | 真实相对路径 |
| 骨架 `.dt-skeleton` | 12px / 1.8 等宽 | `#` 标题行分四级着色（`####` 薄荷 600 / .82） |
| 路径 chip | 11.5px 等宽 | 薄荷色，可换行 |

### 5.3 间距与形状

| 项目 | 值 |
| --- | --- |
| 面板圆角 | 18px（`.panel`） |
| 树卡片 / 骨架 / 文件项圆角 | 10px |
| 分工卡圆角 / 内边距 | 14px / 18px |
| 形态切换外壳 | 10px，按钮 8px |
| 面板内边距 | 24px |
| 树卡片内边距 | 16px 18px |
| 目录层级缩进 | 每级 18px |

### 5.4 动效

| 动效 | 触发 | 说明 |
| --- | --- | --- |
| 形态切换 | 点击「页面 >5 / ≤5」 | 树、导航、计数、详情同时替换，无过渡 |
| 模板切换 | 点击文件项 | 详情替换，`aria-current` 高亮 |
| 移动端选完 | 点击文件项 | 收起列表并平滑滚动到详情 |
| 复制反馈 | 复制按钮 | `studio-toast` + 状态行字节数 |
| 减弱动效 | `prefers-reduced-motion` | 由 `work-hub.css` 全局关闭 |

## 6. 组件清单

| 组件 | 关键类名 / 函数 | 说明 |
| --- | --- | --- |
| 密码门 | `js/gate.js` | 弱隔离，解锁后移除遮罩 |
| 形态切换 | `.dt-mode` `.dt-mode-btn` `setMode()` | 两个按钮，`aria-pressed` 标记当前形态 |
| 目录树 | `#dtTree` `.dt-fs` `.dt-fs-folder` `.dt-fs-file` `parseTree()` `treeList()` | 从纯文本树解析后渲染为嵌套列表，线用 CSS 画 |
| 三份分工 | `.dt-role` `.dt-role-card` `.dt-role-card--focus` `.dt-role-kicker` `.dt-role-list` | 静态三栏：DESIGN 有什么 / SPEC 做成什么样 / TASKS 怎么拆，中间卡高亮 |
| 结构规则 | `#dtRules` `.dt-rules` `renderRules()` | 10 条：三份分工 / 样式只认 spec / 选项统一下拉框 / 设计→任务→验收 / 双端一套壳 / 规模 / 变更留痕 / 代码同改 / demo 镜像 / 页面命名 |
| 命名速查 | `.dt-naming` | 静态说明：变更记录、技术栈规范、归档、根入口 |
| 资料输入 | `#dtProjectName` `#dtStack` `.dt-fields` | 只替换两个占位符，实时刷新树、导航、详情 |
| 整套操作 | `#dtCopyAll` `#dtZip` `bundleText()` `downloadZip()` | 复制当前形态全部骨架 / 下载 ZIP |
| 文件导航 | `#dtNav` `.dt-doc` `renderNav()` | 按目录分组，显示路径与一句用途 |
| 折叠选择器 | `#dtNavToggle` `#dtNavCurrent` `applyNav()` | ≤900px 显示，`aria-expanded` 标记状态 |
| 详情预览 | `#dtDetail` `#dtDetailTitle` `#dtSkeleton` `paintSkeleton()` | 路径、标题、用途、何时写、谁引用、骨架规模、骨架 |
| 占位提示 | `#dtHint` | 未填资料时提示哪些占位符会原样保留 |
| 单份操作 | `#dtCopy` `#dtDownload` | 复制 / 下载当前骨架 |
| 流程说明 | `.dt-steps` `.dt-step` | 五步用法 |

## 7. 按钮与入口

| 按钮 / 入口 | 文案 | 类名 / 选择器 | 行为 | 去向 |
| --- | --- | --- | --- | --- |
| 密码门 | 解锁工作台 | `#gateBtn` | 校验密码 | 本页 |
| 导航 | 工作经验 / 问题排查 / 文档模板 / 命令片段 | `.hub-nav a` | 跳转 | 对应页面 |
| 形态 | 页面 >5 / 页面 ≤5 | `.dt-mode-btn` | 切换目录形态 | 本页 |
| 复制目录结构 | 复制目录结构 | `#dtCopyTree` | 复制纯文本树 | 剪贴板 |
| 整套复制 | 复制整套模板 | `#dtCopyAll` | 复制当前形态全部骨架，含文件分隔 | 剪贴板 |
| 下载骨架 | 下载 docs 骨架 .zip | `#dtZip` | 生成含目录条目的 ZIP | 本机 |
| 折叠选择器 | 选择模板 · 当前文件 · 切换 | `#dtNavToggle` | 展开 / 收起文件列表 | 本页 |
| 文件项 | 路径 + 一句用途 | `.dt-doc` | 切换详情 | 本页 |
| 复制单份 | 复制这份 | `#dtCopy` | 复制当前骨架 | 剪贴板 |
| 下载单份 | 下载 .md | `#dtDownload` | 下载当前骨架 | 本机 |

## 8. 交互与状态

- **形态模型**：`multi`（页面 >5）14 个位置，`design / tasks / spec` 各建文件夹 + `pages/`，`spec/` 下另有一份全局 `设计规范.md`；`slim`（页面 ≤5）11 个位置，三个文件夹都不建，`docs/` 下直接放 `design.md`、`tasks.md`、`spec.md` 和一份 `变更记录.md`。全局设计规范在 slim 形态并入 `spec.md` 第 2 章，因此不单独占位置。页面模板与变更记录都成对（`pair`），切换形态时自动落到对应文件。
- **数据自检**：`js/project-doc-templates.js` 启动时检查 id 重复、未知形态、非法占位符、pair 缺半；失败时控制台报错，状态行显示「模板数据校验失败 N 项」。
- **占位符**：只支持 `{项目名称}` 与 `{技术栈}`；填充后实时替换树、导航、预览、复制与 ZIP。`{技术栈}` 在文件名中取短名（`Java 21 + Spring Boot` → `Java`），为空时回落 `xxx`。
- **空值**：输入为空时保留占位符，并在详情上方提示会原样保留哪些占位符。
- **复制降级**：优先 `BlogStudio.copy`；剪贴板不可用（如 `file://` 限制）时自动下载 `.md` 并在状态行说明。
- **整套复制**：`# 项目 · 文档骨架（N 份 · 形态）` + 10 条文档规则 + N 段（>5 为 14 份 / ≤5 为 11 份）；分隔符为 `===== 文件 n/N：路径 =====`，不使用代码围栏；超过 200 KB 拒绝复制并提示改用 ZIP（multi 全集约 63 KB，slim 约 35 KB）。
- **骨架规模**：详情元信息第三列实时显示当前骨架的行数与 UTF-8 体积（如「400 行 · 20.4 KB」），方便判断把这份贴给 Agent 的量级。
- **ZIP**：store 模式（不压缩），UTF-8 文件名（通用位 `0x0800`），额外写入 `docs/`、`docs/design/` 等目录条目，避免解压器漏建目录。
- **存储**：只写 `v2` 四个字段；读取时逐项校验，损坏则退回默认值。首次加载读取 `v1` 的 `projectName` / `techStack` 填入后删除旧键。
- **错误态**：存储不可用时状态行标红，不阻塞渲染；ZIP 生成失败给出原因。
- **无障碍**：文件导航是 `nav > div > h3 + ul > li > button`（不用 tablist）；文件项用 `aria-current="true"`；树与骨架均可聚焦滚动；折叠按钮有 `aria-expanded` / `aria-controls`；状态行 `role="status"`。

## 9. 响应式

| 断点 | 变化 |
| --- | --- |
| `max-width: 760px` | 目录树与规则改单栏；树卡片撑满并降到最大高 360px |
| `max-width: 900px` | 三份分工改单列；工作台改单栏；导航变「选择模板」折叠器（默认收起）；详情改上分隔线；资料输入单列；流程两列 |
| `max-width: 620px` | 形态按钮撑满；详情元信息三列改单列；树与文件项字号降到 11.5px；流程单列 |

## 10. 内容维护

| 想改什么 | 改哪里 |
| --- | --- |
| 模板骨架正文 | `js/project-doc-templates.js` 的 `items[].skeleton` |
| 全局设计规范（颜色 / 字体 / 按钮 / 按钮下拉框 / 弹层 / 滚动条） | 同文件 `items[]` 中 `id: 'spec-visual'` 的骨架；slim 版在 `id: 'spec-single'` 第 2 章 |
| 页面设计模板（字段 / 场景 / 理由） | `id: 'design-page'`（multi）与 `id: 'design-single'`（slim） |
| 页面规范模板（字段约束 / AC 验收） | `id: 'spec-page'` 与 `id: 'spec-single'` |
| 任务模板（12 条标准包） | `id: 'tasks-page'` 与 `id: 'tasks-single'` |
| 文件路径 / 标题 / 用途 / 何时写 / 谁引用 | 同文件的 `items[]` 字段 |
| 两套目录树文案 | 同文件的 `trees.multi` / `trees.slim` |
| 结构规则 | 同文件的 `rules[]` |
| 命名速查 | `pages/doc-templates.html` 的 `.dt-naming`；变更记录一行由 `renderNaming()` 按形态更新 |
| 首屏标题 / 说明 / chip | `.hub-hero` |
| 三份分工卡文案 | `pages/doc-templates.html` 的 `.dt-role` |
| 页面样式 | `css/doc-templates.css` |
| 整套复制格式 / ZIP 逻辑 | `js/doc-templates.js` 的 `bundleText()` / `zipStore()` |
| 存储版本与迁移 | 同文件的 `STORAGE_KEY` / `migrateV1()` |

## 11. 修改须知

- **关联文件**：`pages/doc-templates.html`、`css/doc-templates.css`、`js/project-doc-templates.js`、`js/doc-templates.js`、`script/studio.js`、`js/gate.js`。
- **模板即产品**：骨架要写成「照着抄就能开发」的可执行规范——design 给字段与场景、spec 给色值/像素/状态/验收、tasks 给 12 条任务包；只保留表头没有示例值的空表不算合格。新增文件必须同时改 `trees`、`items` 与本节文档。
- **占位符契约**：只允许 `{项目名称}` / `{技术栈}`；页面名统一写「页面名称」，避免与占位符语法冲突。骨架内不得出现代码围栏（三反引号），也不得出现其它花括号，否则 `assertKit` 判失败。
- **表格列数**：每张 Markdown 表的表头、分隔行、数据行列数必须一致；改模板后跑一次列数校验。
- **两套形态位置数不同（>5 14 个、≤5 11 个）**：增删模板时同步检查 `modes`、`pair`、目录树和导航计数；≤5 时不要恢复 `design/`、`tasks/`、`spec/` 三个文件夹，全局设计规范并入 `spec.md`。
- **双端一致是硬要求**：`spec-visual` 第 3.5、11 章与 `spec-page` 第 9 章必须保持「颜色令牌 / 按钮 / 弹层 / 滚动条一致，只允许密度与控件高度差异」的口径，改一处要三处同改。
- **选项形态唯一**：点击按钮弹出选项只允许 `spec-visual` 7.3.2 的按钮下拉框。六处口径必须一致：`spec-visual` 7.1 与 7.3.2、`spec-page` 第 7 章、`spec-single` 2.6、`design-page` 第 5 章、`AGENT.md` 第 6 节、`coding` 第 6 章。新增页面时不要引入第二套菜单组件。
- **版本兼容**：存储键有版本号，改数据结构要升版本并处理旧键；v1 草稿只用于迁移两个资料字段。
- **必须同步**：本文件 + [`docs/change.md`](../../change.md) 的「开发文档模板台」小节。
