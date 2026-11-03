# TTBC 短剧分销平台 Prototype：完整需求分析与生成上下文

> **用途**：将本文整体提供给另一个 Agent，使其无需读取原始长对话，也能理解需求形成过程、最终有效口径、已作废口径、页面与交互范围、视觉规范及验收标准，并生成与当前版本要求一致的可交互 HTML Prototype。
>
> **重要定义**：后续提到“需求分析过程”，指从最初会议/产品方案分析开始，经过五步 Prototype Definition、客户调研修订、视觉规范确认和后续导航迭代的全部内容。本文已将这些内容按“最终有效口径优先”重新整理。

---

## 0. Agent 执行摘要（必须先读）

这是一个由 **TikTok Business Center（TTBC）承载的短剧分销商工作台 Prototype**。界面必须为全英文，交付为可独立打开、可分享、可部署到常规静态托管环境的 **单页 HTML**，使用原生 HTML/CSS/JavaScript，不依赖后端。

产品只服务一类主用户：**短剧分销商**。本项目支持“水上分销 / 平台托管分销”：

1. 版权方在 TTDC 选择部分短剧开启分销权限；
2. 分销商在 TTBC 浏览可分销剧目并认领具体 Collection；
3. 分销商在 TTBC 生成、复制或导出分销链接；
4. 分销商去 TTAM，用分销链接圈选可投 Collection 并完成广告决策和投放；
5. TTBC 持续同步链接有效性；链接失效时触发 TTAM 自动停投并回传结果；
6. 分销商回 TTBC 查看有限的分销经营数据、核对正式结算单、主动确认结算，并查看打款日志与异常。

本项目只有三个业务模块：

- **Drama Marketplace**（选剧广场）
- **Distribution Management**（分销管理）
- **Revenue & Settlement**（收益与结算）

下列能力明确不属于本项目：新增准入与签约、TTBC 手动关闭链接、广告 Creative 生产或管理、MVP 人工撮合/定向派单/个性化推荐、复杂角色权限、API、复杂投放分析。

Prototype 必须覆盖正常、空、加载、异常、部分成功、结果未知、链接失效/自动停投、待确认结算、打款异常等演示状态，并通过独立的 Floating demo controller 切换。该控制器是演示工具，不属于正式产品 UI。

当前最终导航规范：

- 黑色顶部全局导航，高度 64px；标题为 **TikTok: Business Center**。
- 左上保留 **68×36 的 App Grid + 28px 头像组合按钮**，但本 Prototype 不展开菜单，点击仅显示演示反馈。
- 左侧全局导航常驻展开，宽度 240px。
- **Distribution Center 位于 Creatives 分组下**，作为可展开一级项。
- 一级项下默认展开三个二级页面：Drama Marketplace、Distribution Management、Revenue & Settlement。
- 二级选中态：左右各 8px 留白、浅青色背景 `#DFF3F2`、4px 圆角、无左侧黑色竖线。
- 顶部与侧栏应以随附的 `navigation-reference.png` 为视觉基准；使用风格一致的正式 SVG 图标，禁止使用 emoji 或 Unicode 符号近似图标。

---

# 1. 项目来源与证据背景

## 1.1 输入材料类型

需求分析综合了以下项目材料：

- 两次分销平台方案会议记录；
- 一份分销产品方案；
- 一份客户调研总结及其中可确认的研究结论；
- 后续围绕角色、任务、对象模型、异常状态和 Prototype Definition 的多轮确认。

本文已经吸收这些材料中被后续确认或修订的结论。另一个 Agent 不需要定位原始材料，也不应在产物中暴露材料来源、链接或访问方式。

## 1.2 证据边界

- 两次方案会议的详细内容已用于初始分析；
- 客户调研的修订以可确认的调研总结为准；
- 无法直接核验的会议原文没有被假装读取；
- 本文中的“最终有效口径”优先于早期讨论和未确认假设。

## 1.3 初始系统结论

TTBC 不是所有分销能力的承载平台，而是：

> **分销商统一工作台 + 跨系统编排层**。

系统职责大致分布：

- TTDC：版权方侧剧目与分销授权管理，决定剧目是否开放分销，也承载手动关闭分销链接等版权方动作。
- TTBC：分销商选剧、认领、链接生成/复制/导出/监控、有限经营数据、结算核对与打款日志入口。
- TTAM：广告决策、Creative Library 触点、投放创建与管理；接收分销链接用于圈选可投 Collection；收到失效信号后自动停投或调整受影响广告。
- 数据服务：提供归因或分销数据。
- 结算与支付系统：承接正式结算及打款执行。

---

# 2. 最终产品边界

## 2.1 本项目包含

仅包含以下三个模块：

1. Drama Marketplace
2. Distribution Management
3. Revenue & Settlement

## 2.2 本项目不包含

### 已有能力复用，不新增/不改造

- 准入与签约：完全复用现有产品能力，仅作为使用本系统的外部前置条件。

### 明确不由 TTBC 承载

- 版权方开启/关闭分销权限：在 TTDC。
- 手动关闭分销链接：只在 TTDC 支持，TTBC 与此操作无关。
- 广告 Creative / 素材生产：TTBC 不提供；用户在 TTAM 触点进入独立 Creative Library 生成或上传。
- 广告投放分析：CTR、CPC、CPA、ROI 等留在 TTAM，不复制到 TTBC。

### MVP Non-goals

- 人工撮合、定向派单
- 运营排序策略
- 个性化推荐
- 批量认领
- 复杂经营分析
- 复杂角色权限体系
- API
- 内部运营、财务等角色使用本平台的独立流程

## 2.3 历史命名修订

- 早期名称“我的分销”已作废。
- 最终统一为 **Distribution Management / 分销管理**。

---

# 3. 主用户与使用场景

## 3.1 只有一类产品主用户

**Short-drama distributor / 短剧分销商**。

不要在当前产品中为运营、财务、版权方或平台内部人员创建独立角色、权限或工作台。

## 3.2 真实用户形态

产品层面只有一类角色，但真实客户通常是组织型分销商：

- 内部可能有剪辑、投放、运营等分工；
- 多个团队可能对应不同 Business Center；
- 不同 BC 并行经营并独立结算；
- MVP 仍以同一 BC 下的共同使用者承接，不做复杂细粒度权限。

## 3.3 典型行为特征

短剧分销商不是普通广告主或个人推广达人，而是具备选品、内容判断、规模化投放与经营核账能力的专业经营主体。典型使用习惯：

- 多剧并行经营；
- 快速测试、快速换剧；
- 批量生成、复制、导出链接；
- 重视链接是否持续有效，以及失效原因；
- 重视自动停投是否执行成功；
- 重视结算金额确定性和打款日志；
- 选剧会查看 AI/真人、本土/翻译、题材、集数、语言、上线时间、版权方、热度/榜单等信息；
- 希望直接在线播放短剧，常用约 2 倍速；
- 重点查看前 5–10 集、付费卡点前后和中后段内容质量。

## 3.4 业务模式

本需求支持 **“水上分销 / 平台托管分销”**：

- 版权方在 TTDC 开放一部分剧目；
- 分销商在 TTBC 自由浏览和认领；
- MVP 是完全自由、自主的 outsourcing 模式；
- 不包含平台人工撮合或运营策略。

---

# 4. 核心业务闭环

1. 版权方在 TTDC 为 Contract/Collection 开启分销权限。
2. 短剧分销商进入 TTBC 的 Drama Marketplace。
3. 用户筛选、查看剧目详情、在线播放内容并判断是否适合分销。
4. 用户选择一个或多个具体 Collection，并执行认领。
5. 认领成功后，Distribution Management 可生成对应分销链接。
6. 用户单条复制、批量复制或导出链接。
7. 用户前往 TTAM，在投放创建中填写分销链接，以圈选可投 Collection。
8. 用户在 TTAM 完成广告 Creative 选择/生成、广告决策与投放。
9. TTBC 持续显示链接状态和变化原因。
10. TTDC 状态变化导致链接失效时：TTBC 更新链接状态 → 通知 TTAM 自动停投 → TTAM 回传执行结果。
11. 用户在 Revenue & Settlement 查看有限的分销数据链路。
12. 正式月度结算单生成后，用户主动核对并明确确认。
13. 确认后进入打款流程，用户查看打款日志与失败原因。

---

# 5. 核心对象模型

## 5.1 Contract

- Contract 对应原片 / 一部剧，是父级对象。
- Contract ID 是原片级业务标识。
- 一个 Contract 可以在不同地区生成多个语言/市场版本。

## 5.2 Collection

- Collection 是 Contract 的子集。
- 一个 Collection 对应一个具体地区语言版本。
- 国家/地区和语言是 Collection 属性。
- 同一 Contract 可有多个 Collection。

## 5.3 分销关系与链接标识

最终业务 identifier：

> **Collection ID × 当前分销商 / 当前 Business Center**

说明：

- 地区、语言不再额外加入 identifier，因为它们已经是 Collection 属性。
- 一次认领最终落到一个或多个具体 Collection。
- 同一个 Collection × 当前 BC 应只有一条有效分销关系/链接；链接生成应幂等，重复提交不能生成重复链接。

## 5.4 内容与素材边界

- “短剧内容预览”属于选剧决策对象，TTBC 应提供。
- “广告 Creative”不属于 TTBC，由 TTAM 中的 Creative Library 承接。

## 5.5 搜索与显示优先级

- Contract ID、Collection ID 均可搜索和复制。
- 默认视觉层级不应压过剧名、国家和语言。
- 标题/剧名、市场、语言是用户更常用的识别信息。

---

# 6. 三个模块的目标、JTBD 与完成点

## 6.1 Drama Marketplace

### 模块目标

帮助分销商发现可分销剧目、理解内容质量与可用市场版本，并认领具体 Collection。

### 核心 JTBD

> 当我准备寻找新的短剧投放机会时，我需要快速筛选并深度预览剧目，判断其题材、内容质量、市场版本和分成条件，从而认领适合测试的具体 Collection。

### 完成点

- 完成点是 **成功认领可分销对象（Collection）**。
- 不是直接创建广告投放。

### 主要能力

- 按 Contract 聚合展示剧目卡片；
- 搜索剧名、Contract ID、版权方；
- 筛选语言、格式（Live action / AI animation）、来源（Local original / Localized）等；
- 显示题材、集数、语言、上线时间、版权方、热度/榜单等选剧信息；
- 详情 Drawer；
- 在线内容预览；
- 2× 播放速度模拟；
- 选集，重点体现前 5–10 集、付费卡点和中后段；
- 展示 Contract 下各 Collection 的国家、语言、Collection ID、分成比例与可认领状态；
- 用户可多选 Collection 后确认认领；
- 认领结果需要支持部分成功或结果未知。

### 不做

- 不在 Marketplace 直接创建广告；
- 不提供 Creative；
- 不做个性化推荐或运营排序策略；
- 不做批量认领主流程。

---

## 6.2 Distribution Management

### 模块目标

帮助分销商取得、批量处理并持续监控分销链接，包括链接状态与 TTAM 自动停投结果。

### 核心 JTBD

> 当我已经认领 Collection 后，我需要批量生成、复制或导出分销链接，并持续知道链接是否有效、为何失效、受影响广告是否已自动停投，从而避免继续给失效剧目投放。

### 完成点

- 用户取得可用于 TTAM 的链接；
- 可以批量复制或导出；
- 可以持续监控状态和异常恢复。

### 两个视图

#### Link view（默认）

- 高频默认视图；
- 一行对应 `Collection × 当前 BC` 的分销链接；
- 优化批量生成、复制、导出和异常处理；
- 用户已明确确认 Link view 为默认。

#### By drama

- 按 Contract 聚合；
- 展开查看其 Collection / links；
- 用于理解原片覆盖市场、预览剧目、查看关系及进入相关数据/结算。

### P0 能力

- Batch generate links
- 单条复制
- 批量复制
- 完整导出
- 搜索 Contract ID、Collection ID、剧名、链接
- 链接状态监控
- 状态变化原因
- 自动停投状态与结果
- 链接详情与状态历史
- Open TTAM

### TTAM 依赖

1. 在 TTAM 投放时填写分销链接，用于圈选可投 Collection。
2. 链接失效后，TTAM 必须停止或调整受影响投放，并回传结果。

### 状态必须拆开建模

“Link invalid”与“Auto-pause result”是两个不同状态：

- 链接可能已失效；
- 自动停投可能 pending / processing / complete / partial / failed / unknown。

不要把两者合并成一个模糊状态。

### 手动关闭修订

- TTBC 不提供手动关闭链接。
- 手动关闭在 TTDC。
- 任何早期版本中出现的 TTBC 手动关闭按钮或描述均作废。

### 跨系统返回体验

从 TTBC 去 TTAM 或其他模块后返回时，应恢复：

- 原对象
- 原 View
- 原筛选条件
- 原滚动位置
- 必要的选中上下文

Prototype 可通过 modal/toast 模拟该链路，但文案要说明 context preserved。

---

## 6.3 Revenue & Settlement

### 模块目标

将“持续更新的经营核对数据”与“正式应收结算单”分开：

- Distribution data：有限数据披露和逐级核对；
- Settlement management：正式结算单、主动确认、打款日志和异常处理。

## 6.3.1 Distribution data

### 用户路径

> 总览 → Contract 剧目 → Collection/链接 → 日期 × 链接明细 → 完整导出

### 最细建议粒度

- 日期 × 分销链接

### MVP 展示数据

- Estimated distribution revenue
- 可选 Attributed GMV
- Contract 数量
- Collection/链接数量
- 更新时间 / 数据延迟
- 必要状态（Ready / Adjusted 等）

### 明确不展示

- CTR
- CPC
- CPA
- ROI
- 其他 TTAM 广告效果分析指标

### 数据口径

- Distribution data 是持续更新、可能延迟的经营核对数据；
- 不能暗示为最终结算金额；
- 页面应显著提示：最终应收金额以 settlement statement 为准。

## 6.3.2 Settlement management

### 用户路径

> 周期结算单 → 剧目拆分核对 → 明确确认 → 打款记录与日志 → 异常恢复

### 强制确认

- 结算确认必须是用户主动执行、明确可见、不可省略的动作；
- 不能由查看详情、下载账单或进入页面隐式替代；
- 如果结算金额或版本发生变化，应重新要求确认。

### 主要状态

- Pending confirmation
- Processing / queued for payout
- Paid
- Payout failed

### 打款异常

需显示：

- 失败状态
- 失败原因（例如 beneficiary name mismatch）
- 发生时间
- 后续建议/日志

### 结算组织口径

客户调研表明，月度按主/子 BC 和剧目拆分账单基本可满足核账。Prototype 可按 Business Center、周期、Contract 做拆分。

---

# 7. 六条关键流程与异常恢复

## 流程 1：选剧、预览、选择 Collection、认领

1. 浏览 Marketplace；
2. 筛选与搜索；
3. 打开 Contract 详情 Drawer；
4. 播放内容预览、切集、2×；
5. 选择可认领 Collection；
6. 打开 Confirm claim Modal；
7. 明确认领；
8. 展示逐项结果。

异常：部分成功、结果未知、版本在确认过程中变化。

## 流程 2：批量生成链接

1. 进入 Distribution Management / Link view；
2. 点击 Generate links；
3. 选择 BC 和已认领但未生成链接的 Collection；
4. 提交生成；
5. 按 Collection 逐项反馈。

要求：

- 幂等；
- 不能因重试生成重复链接；
- 需要区分 generated / already existed / failed / unknown。

## 流程 3：复制、批量复制、导出、前往 TTAM

- 单条 Copy；
- 勾选多条 → Copy selected；
- Export；
- Open TTAM；
- 跳转前说明链接用于识别可投 Collection；
- 返回后恢复上下文。

## 流程 4：TTDC 状态变化 → 链接失效 → TTAM 自动停投

1. TTDC 关闭分销、剧目下架、授权到期等；
2. TTBC 将链接设为 invalid；
3. TTBC 显示失效原因和更新时间；
4. TTAM 自动停投；
5. 回传完成/部分完成/失败/未知；
6. 用户查看受影响广告与历史。

该流程是跨系统 P0。

## 流程 5：分销数据下钻与完整导出

1. Distribution data 总览；
2. 按 Contract 查看；
3. 下钻到 Collection/link；
4. 查看日期 × link 明细；
5. 导出完整数据。

## 流程 6：结算核对、确认、打款日志

1. 查看月度结算单；
2. 打开详情，查看 BC、周期、币种、Contract 拆分和最终金额；
3. 点击 Confirm settlement；
4. 二次确认 Modal；
5. 状态进入 Processing；
6. 查看打款日志；
7. 异常时查看失败原因并恢复。

---

# 8. Prototype 页面清单

## 8.1 左侧全局导航

常驻展开，至少模拟以下分组与入口：

- Business agent
- Platforms
  - Ads Manager
  - Business Suite
- Creatives
  - TikTok One
  - Creative Library
  - Distribution Center（可展开，默认展开）
    - Drama Marketplace
    - Distribution Management
    - Revenue & Settlement
- Connection
  - Leads Center
  - Events Manager
- Planning
  - Apps & Agents
- Settings
  - Billing
  - Business Settings
- All tools

非本 Prototype 入口点击后以 toast 告知 outside this prototype，不做僵尸按钮。

## 8.2 主页面/视图

Floating controller 至少可以切换以下 5 个页面视图：

1. Drama Marketplace
2. Distribution Management · Link view
3. Distribution Management · By drama
4. Revenue & Settlement · Distribution data
5. Revenue & Settlement · Settlement management

## 8.3 Drawer / Modal

- Drama detail Drawer
- Claim confirmation Modal
- Claim results Modal
- Generate links Drawer
- Generation results Modal
- Link details Drawer
- TTAM handoff Modal
- Export Drawer
- Contract/link-level data Drawer
- Daily link detail Modal
- Settlement statement Drawer
- Confirm settlement Modal

---

# 9. 全局演示状态

Floating demo controller 是深色悬浮面板，与正式产品 UI 明显区分。至少包含：

1. Normal
2. Empty
3. Loading
4. Error
5. Partial success
6. Result unknown
7. Link invalid & auto-pausing
8. Settlement pending confirmation
9. Payout exception

要求：

- 控制器可折叠；
- 可切换 Page 和 Global state；
- Reset；
- Copy demo link（可模拟）；
- 切换后页面内容真实变化，不仅改标签；
- controller 不应被误认为产品正式功能。

---

# 10. 批量操作与状态设计规则

## 10.1 逐项反馈

批量认领/生成/复制等操作不能只给一个总成功 toast，需按对象反馈：

- Success
- Existing / skipped
- Failed
- Unknown / confirming

## 10.2 结果未知

结果未知不是失败：

- 告知系统正在确认；
- 提醒不要立即重复提交；
- 保留重新查询或恢复路径。

## 10.3 部分成功

- 明确成功、已存在、失败数量；
- 可查看逐项明细；
- 成功项应可继续复制/导出。

## 10.4 幂等

链接生成、认领等跨系统操作需按业务 key 保证幂等，Prototype 文案要体现这一心智。

---

# 11. 视觉与 UI 规范

## 11.1 界面语言

- 所有产品 UI 使用英文。
- 文案使用 sentence case。
- CTA 准确描述动作结果，例如 Generate links、Confirm settlement、Open TTAM。

## 11.2 视觉参考体系

![顶部与左侧导航视觉参考](navigation-reference.png)

- 随附文件 `navigation-reference.png` 是顶部与左侧导航的最终视觉基准；
- 主内容区遵循高密度企业工具的组件语言：紧凑表格、筛选栏、Drawer、Modal、下划线 Tabs；
- 优先使用随附件提供的正式 SVG 资产；若未提供，则使用风格一致的矢量图标，不得使用 emoji 或 Unicode 符号代替；
- 只需依赖本文和随附视觉附件即可完成 Prototype。

## 11.3 顶部导航最终规范

- 高度：64px
- 背景：`#121415`
- 左侧保留 68×36 App Grid + Avatar 组合 pill：
  - App Grid 为 production `BcIconAppGrid`；
  - Avatar 28×28，青色；
  - 本 Prototype 不需要展开菜单，点击给 toast 即可；
- 标题：官方 TikTok wordmark / “TikTok:” + **Business Center**；
- 右侧：Search / Help / Notifications 的统一矢量图标按钮；
- Business portfolio pill，例如 `Shike Interactive (Admin)`；
- 不用 emoji、Unicode 字符近似图标。

## 11.4 左侧导航最终规范

- 宽度：240px
- 白色背景，右侧 1px 分隔线
- 常驻展开全局导航，不把分组折叠到左上角菜单中
- section label：12px 灰色
- 一级行：40px，图标 20px，图标到文字 12px
- Distribution Center 位于 Creatives 下，一级项使用 Share/Distribution 语义的统一矢量图标与展开箭头
- 默认展开三个二级页面
- 二级项：
  - 不显示图标
  - 高度 40px
  - 文字与父级标题列对齐（约左侧 52px）
  - 当前选中项左右各 8px 留白
  - 浅青背景 `#DFF3F2`
  - 4px 圆角
  - 无左侧黑色竖线
- 父级可点击展开/收起，箭头旋转

## 11.5 主内容区

- 高密度企业工具风格
- 表格紧凑、筛选紧凑
- Primary 使用青绿色
- 灰阶容器和 1px 边界
- Tabs 使用下划线 active
- Drawer 从右侧打开
- Modal 居中
- 不使用泛 SaaS 大圆角卡片、大面积渐变或装饰性阴影

## 11.6 素材

- 使用虚构剧目，不使用真实业务剧目。
- 当前虚构 Contract：
  - The Last Clause
  - A House by the Tide
  - Moonblade Oath
  - The Physician at Midnight
- 海报应无文字，业务名称由 HTML 叠加。

---

# 12. 当前 Prototype 的示例数据语义

可使用以下样例结构，但所有数字仅作为 Prototype 示例，不代表真实业务数据。

## Contract 示例

- `CTR-902184` The Last Clause
- `CTR-781230` A House by the Tide
- `CTR-550361` Moonblade Oath
- `CTR-340918` The Physician at Midnight

## Collection 示例

- `COL-US-18421` / United States / English
- `COL-GB-18422` / United Kingdom / English
- `COL-BR-77310` / Brazil / Portuguese
- `COL-MX-77311` / Mexico / Spanish
- `COL-US-55001` / United States / English
- `COL-JP-55002` / Japan / Japanese
- `COL-TH-34001` / Thailand / Thai
- `COL-ID-34002` / Indonesia / Indonesian

## 链接状态示例

- Active
- Invalid — Distribution disabled by the rights holder
- Status unknown — Waiting for TTAM status confirmation

## Auto-pause 示例

- No action needed
- Auto-pausing
- Auto-pause complete
- Partially paused
- Auto-pause failed
- Result unknown

## Settlement 示例

- Pending confirmation
- Processing
- Paid
- Payout failed — beneficiary name mismatch

---

# 13. 交互实现要求

## 13.1 技术形态

- 单个 HTML 文件
- CSS 和 JavaScript 内联
- 原生 JS，不用 React/Vue/构建工具
- 多页面用 hash 路由
- 可直接 `file://` 打开
- 可部署到常规静态托管环境
- 图片可以使用稳定资源 URL 或正确静态资产引用；不要依赖会过期的临时生图链接

## 13.2 禁止僵尸按钮

所有视觉上可点击的元素必须有：

- 真实交互；或
- 路由；或
- Drawer / Modal；或
- toast 反馈。

不做无 handler 的 button 或无效 anchor。

## 13.3 核心路径必须可运行

至少验证一条：

> 打开剧目详情 → 选择可认领 Collection → 打开 Confirm claim Modal

以及导航路径：

> Distribution Center 二级菜单 → Distribution Management → Link view

## 13.4 页面返回与上下文

模拟 TTAM 跳转后，返回应提示恢复原 Distribution link 上下文。

---

# 14. 最终验收清单

## 产品边界

- [ ] 只有三个主模块
- [ ] 不新增准入与签约
- [ ] 不提供 TTBC 手动关闭链接
- [ ] 不提供广告 Creative
- [ ] 不复制 TTAM 投放分析

## 对象模型

- [ ] Contract 是父级
- [ ] Collection 是地区语言版本
- [ ] identifier = Collection ID × 当前 BC/分销商
- [ ] 地区和语言是 Collection 属性

## Marketplace

- [ ] Contract 卡片
- [ ] 多维筛选
- [ ] Drawer 内容预览
- [ ] 2× 播放/选集
- [ ] Collection 多选认领
- [ ] 明确认领确认
- [ ] 部分成功/结果未知

## Distribution Management

- [ ] Link view 默认
- [ ] By drama 可切换
- [ ] Batch generate P0
- [ ] 单条复制、批量复制、导出 P0
- [ ] 链接状态与原因
- [ ] Auto-pause 独立状态
- [ ] TTAM handoff
- [ ] 无 TTBC manual close

## Revenue & Settlement

- [ ] Distribution data 与 Settlement management 分开
- [ ] 总览→剧目→链接→日期明细→导出
- [ ] 数据提示非最终结算金额
- [ ] 正式结算单强制主动确认
- [ ] 打款日志与异常

## 状态

- [ ] Normal
- [ ] Empty
- [ ] Loading
- [ ] Error
- [ ] Partial success
- [ ] Result unknown
- [ ] Link invalid & auto-pausing
- [ ] Pending settlement confirmation
- [ ] Payout exception

## 视觉

- [ ] 全英文
- [ ] 顶栏 64px，TikTok: Business Center
- [ ] 左上保留 68×36 App Grid + Avatar pill，但不展开菜单
- [ ] 左侧全局导航 240px 常驻展开
- [ ] Distribution Center 位于 Creatives
- [ ] 三个页面为二级菜单
- [ ] 二级 active 为 `#DFF3F2`、左右 8px、4px radius、无左侧线
- [ ] 使用风格统一的正式矢量图标，不用 Unicode 近似
- [ ] Floating controller 明显区别于产品 UI

## 技术与质量

- [ ] 单页 HTML
- [ ] 所有按钮有反馈
- [ ] hash 路由正常
- [ ] Drawer/Modal 正常
- [ ] 桌面端无横向溢出
- [ ] 使用浏览器渲染检查器自检
- [ ] 0 blocker / 0 error / 0 warning（信息级重复封面提示可接受）

---

# 15. 已作废或容易误用的早期口径

另一个 Agent 必须避免以下错误：

1. **Distribution Center 放在 Connection 下**：已作废；最终放在 Creatives 下。
2. **把全局导航收进左上角 Workspace 菜单**：已作废；最终全局导航常驻展开在左侧。
3. **左上角完全移除 App Grid**：已作废；最终保留 App Grid + Avatar pill，但不放菜单。
4. **顶栏显示 Distribution Center**：已作废；最终标题为 TikTok: Business Center。
5. **二级选中项使用全宽灰底 + 左侧黑线**：已作废；最终为左右留白浅青圆角块，无左线。
6. **我的分销**：已作废；统一为 Distribution Management。
7. **TTBC 手动关闭链接**：错误；只在 TTDC。
8. **地区是链接 identifier**：错误；地区/语言属于 Collection 属性，identifier 是 Collection ID × BC/分销商。
9. **Marketplace 直接创建投放**：错误；Marketplace 完成点是认领，投放在 TTAM。
10. **TTBC 提供广告素材**：错误；素材在 TTAM 的 Creative Library。
11. **核对收款完全不提供数据**：已修订；可以提供简单 Distribution data，但必须控制范围并与正式结算分开。
12. **结算确认可由浏览/下载替代**：错误；必须显式主动确认。
13. **链接失效与自动停投是一个状态**：错误；必须分开。
14. **批量生成、批量复制、导出是 P1**：错误；均为 P0。
15. **分销管理默认 By drama**：错误；默认 Link view。
16. **为运营/财务做独立角色**：不在本次范围。

---

# 16. 附件与参考方式

## 16.1 必备附件

- `navigation-reference.png`：最终顶部与左侧导航视觉参考；
- 本 Context 文档：产品范围、对象模型、页面、流程、状态与验收标准的唯一文字基准。

## 16.2 可选附件

- 当前版本的 `distribution-center-prototype.html`：若与本文一起提供，优先回读并增量修改；
- 虚构短剧海报素材：若未提供，可生成统一风格、无文字的虚构封面。

## 16.3 使用原则

- 不需要访问任何额外文档、域名、代码仓库或专用系统；
- 若附件与本文冲突，以本文的“最终有效口径”和“已作废口径”章节为准；
- 导航的具体层级、尺寸、选中态和图标风格同时以本文与 `navigation-reference.png` 为准。

---

# 17. 可直接复制给另一个 Agent 的任务指令

```text
请基于随附的《TTBC 短剧分销平台 Prototype：完整需求分析与生成上下文》生成一个全英文、可点击交互、可分享、可部署到常规静态托管环境的单页 HTML Prototype。

执行时必须：
1. 以文档中的“最终有效口径”为准，不得恢复“已作废口径”。
2. 产品只包含 Drama Marketplace、Distribution Management、Revenue & Settlement。
3. 严格实现 Contract → Collection 对象层级，以及 Collection ID × 当前 BC/分销商的分销关系标识。
4. 实现 Link view 默认、Batch generate P0、复制/批量复制/导出 P0、链接失效与 TTAM 自动停投的独立状态。
5. 实现 Distribution data 的“总览→剧目→链接→日期明细→完整导出”，并与正式结算单分开。
6. Settlement confirmation 必须是明确主动操作。
7. 实现 Floating demo controller，切换 5 个视图和 9 种全局演示状态。
8. 导航严格遵循最终规范：64px 黑色顶栏、TikTok: Business Center、68×36 App Grid + Avatar pill（无菜单）、240px 常驻全局侧栏、Distribution Center 位于 Creatives、三个二级页、浅青圆角 active。
9. 以随附 `navigation-reference.png` 为导航视觉基准，优先使用随附或风格一致的通用矢量 SVG，不用 emoji 或 Unicode 图标替代。
10. 所有可见按钮必须有真实交互、路由、弹层或 toast，不得有僵尸按钮。
11. 单 HTML 文件、原生 JS、hash 路由、无后端依赖。
12. 使用虚构短剧和示例数据，不使用真实业务剧目。
13. 完成后进行桌面浏览器渲染检查和一条核心路径验证；修复影响阅读或交互的问题后再交付。

在开始写代码前，请先把文档中的最终边界、关键对象、三模块完成点、六条流程、状态矩阵和导航验收标准整理成内部检查清单；不要跳过或自行扩展范围。
```

---

# 18. 当前产物复用说明

推荐的 HTML 文件名：

- `distribution-center-prototype.html`

如果该 HTML 与本文一起作为附件提供，另一个 Agent 应先完整回读现有文件并增量修改，不要从头重造；若没有提供 HTML，则按本文和 `navigation-reference.png` 完整重建。

参考版本的已验证范围：

- 桌面端渲染正常；
- 顶栏 App Grid + Avatar pill 尺寸正确；
- 标题为 Business Center；
- Distribution Center 位于 Creatives 下并默认展开；
- 三个二级页面路由可用；
- 二级选中态左右 8px 留白、浅青背景、4px 圆角、无左侧线；
- 核心认领路径可进入 Confirm claim Modal。
