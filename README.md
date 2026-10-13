# 股票 / 公司分析工作流（stock-analysis v1.1.0 + corp-investigation v1.1.0）

一套面向 **A股 / 港股 / 美股** 的股票多维评估 + 企业关联穿透调查的可复用技能组合，由 WorkBuddy 用户构建并分享。

- **stock-analysis**（v1.1.0）：从财务（价值锚）、市场情绪（价格动能）、情景推演三维度评估个股，给出短/中/长线操盘建议；**每次分析前先询问持仓状态与成本价**，据此输出**止盈止损价位点**（已持仓）或**建仓策略**（未持仓）；默认生成优雅单文件 HTML 报告（红涨绿跌、评级驱动配色、颜色条全览、手机适配、可打印）。
- **corp-investigation**（v1.1.0）：企业关联穿透调查方法论（7 阶段 SOP、不可能三角、输血通道、隐性关联六法、红旗信号库、合规边界），供 stock-analysis 在风险栏联动筛查"实控人掏空 / 财务造假"信号。

> 两个技能**必须一起安装**，stock-analysis 会在风险栏调用 corp-investigation 的红旗库。
> **许可证**：CC BY-NC 4.0（署名—非商业），详见 `LICENSE`。

---

## 一、本次 v1.1.0 更新要点

- **评级词表统一**：综合评级收敛为唯一一套 4 档（积极配置 / 中性 / 中性偏谨慎 / 回避），与"评级驱动配色"严格一一对应，消除旧版别名混用（强烈关注/可配置/谨慎持有）。
- **持仓问询 + 止盈止损价位（新功能）**：分析前用 `AskUserQuestion` 确认「已持仓 / 未持仓」并追问成本价；报告第八节据此给出**止损价、分批止盈价、持仓动作**（已持仓）或**建仓区间、触发条件、初始仓位上限**（未持仓）。
- **颜色条全览（新可视化）**：第四节省以细线标注 52 周低/支撑/筹码成本/压力/52 周高 五档关键价位，并标注当前价格所在区域（低位区/中位区/高位区），一眼看清估值位置。
- **标准大纲统一**：Markdown 源与 HTML 交付件共用同一 10 节大纲，杜绝结构漂移。
- **预交付核对清单**：交付前自动核对红涨绿跌一致性、评级配色一致性、评分自洽、持仓分支完整性等 7 项。
- **合规强化**：两技能均加入证券合规要点（不构成误导性陈述、不传播未公开信息、不用于造谣/操纵）；新增 `version / author / license` 字段与 `LICENSE` 文件。

---

## 二、安装方式

### 方式 A：手动复制（最简单，推荐小范围分发）
把本包里的两个目录整体复制到用户级技能目录：

```bash
# macOS / Linux
cp -r stock-analysis   ~/.workbuddy/skills/
cp -r corp-investigation ~/.workbuddy/skills/

# Windows (PowerShell)
Copy-Item -Recurse stock-analysis    $env:USERPROFILE\.workbuddy\skills\
Copy-Item -Recurse corp-investigation $env:USERPROFILE\.workbuddy\skills\
```

重启 / 新建 WorkBuddy 会话后即可触发。

### 方式 B：仓库安装（适合公开发布、可版本化）
本包已发布到公开仓库，其他人用技能生态命令安装：

```bash
# 简写（GitHub 仓库）
npx skills add gschenhaojie/workbuddy-stock-analysis
# 或完整 URL
npx skills add https://github.com/gschenhaojie/workbuddy-stock-analysis
```

或在 WorkBuddy 对话中说"**安装技能 stock-analysis**""**安装技能 corp-investigation**"。

---

## 三、怎么用

在 WorkBuddy 对话中直接说（触发词已内置）：

- "用 stock-analysis 框架分析 002045 国光电器"
- "给 XXX 股票做个估值和操盘建议"
- "查一下 YYY 公司有没有实控人掏空 / 关联交易输送嫌疑"

技能会：先**询问持仓状态与成本价** → 拉取最新行情/财报/资金流（WebSearch）→ 三维度评估 → 联动 corp-investigation 红旗筛查 → 按持仓分支给止盈止损价或建仓策略 → 输出 Markdown 定稿 → 渲染优雅 HTML（主色跟随综合评级，含颜色条全览）。

> 默认生成的 HTML 报告会自动在 WorkBuddy 预览面板打开，方便即时验收版面与配色；如只需落盘不打开，可在对话中说明。

**综合评级 → 主色映射**（HTML 自动联动）：

| 评级 | 主色 |
|---|---|
| 积极配置 | 蓝 `#1d4ed8` |
| 中性 | teal `#0f766e` |
| 中性偏谨慎 | 琥珀 `#b45309` |
| 回避 | 红 `#b91c1c` |

> 价格涨跌仍用红涨绿跌（A股惯例），与评级配色正交、互不影响。

**报告标准大纲（HTML / MD 一致，10 节）**：
一、一句话结论 · 二、财务角度评估 · 三、🚩 红旗联动筛查 · 四、市场情绪评估 + 颜色条全览 · 五、下阶段方向 · 六、操盘建议 · 七、综合评级与评分 · 八、持仓状态与止盈止损价位 · 九、关键观察点 · 十、免责声明。

---

## 四、文件清单

```
stock-analysis-workflow/
├── LICENSE                              # CC BY-NC 4.0 许可
├── README.md                           # 本说明
├── 分享指南.md                          # 转发话术 + 安装提示词 + 版本内容说明（发给别人安装用）
├── stock-analysis/                     # 技能①：股票多维评估 v1.1.0
│   ├── SKILL.md                        #   框架 + 触发词 + 评级配色 + 持仓问询 + 核对清单 + 交付纪律
│   └── references/
│       └── report-template.html        #   可复制的优雅 HTML 模板（含 4 档评级 CSS + 颜色条全览 + 持仓价位模块）
├── corp-investigation/                 # 技能②：企业关联穿透调查 v1.1.0
│   └── SKILL.md                        #   7阶段SOP + 红旗库 + 数据来源优先级 + 合规边界
└── samples/
    └── 样例输出-国光电器002045.html     #   真实产出样例（评级=中性偏谨慎，对应琥珀配色；含颜色条全览）
```

---

## 五、依赖与前提

- **联网**：技能需通过 WebSearch / WebFetch 拉取实时行情、财报、资金流与公告；离线环境会退化并声明口径。
- **平台**：WorkBuddy 用户级技能目录 `~/.workbuddy/skills/`（macOS / Linux / Windows 均支持）。
- **无第三方依赖**：纯提示词 + 单文件 HTML 模板，不调用外部 API Key。

---

## 六、免责声明

本工作流基于公开信息与分析模型生成，含【事实】与【推测】两类内容，推测部分不构成投资建议。所有价格为分析时点数据，实时行情可能已变化。据此操作风险自担，本作品不构成任何买卖要约、招揽或法律意见。企业调查部分仅用于研究 / 风控，不输出法律定性，不得用于造谣、市场操纵或传播未公开信息。
