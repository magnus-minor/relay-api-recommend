# AI API 中转站推荐与选站指南（Claude / GPT / Gemini API中转）

> **English:** A Chinese-language guide to choosing an AI API relay (中转站 / relay api / api proxy) for Claude, GPT and Gemini: selection checklist, risk warnings, how to self-check a relay for model downgrades, and per-site data cards linked to [Folkbench](https://folkbench.com/?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend). Issues for new site submissions and pitfall reports are welcome.

> [!IMPORTANT]
> **披露：排序遵循 Folkbench 站点规则，包含合作等级和运营加权；部分数据为展示样例。**
> 本仓库的分站列表顺序与 Folkbench 中转站列表页一致，数据以 Folkbench 页面当前显示为准。任何列表都不能替代你自己的小额试用。

**目录**

- [一、选站标准](#一选站标准)
- [二、风险提醒](#二风险提醒)
- [三、如何自己检测一个中转站](#三如何自己检测一个中转站)
- [四、分站数据（来自 Folkbench）](#四分站数据来自-folkbench)
- [五、需谨慎](#五需谨慎)
- [六、Folkbench Switch 桌面端](#六folkbench-switch-桌面端)
- [七、FAQ](#七faq)
- [八、申请收录 / 踩坑报告](#八申请收录--踩坑报告)
- [更新日志](#更新日志)

---

## 一、选站标准

挑 API 中转站，按下面五项逐条过一遍，任何一项答不上来，就先别加钱。

| 维度 | 要看什么 | 怎么判断 |
|---|---|---|
| **模型是否降级 / 掺假** | 你付 Opus / GPT 旗舰的钱，拿到的是不是同一个模型 | 用固定题目和模型指纹探针对照官方回答；只看「能回话」不够，要看推理深度、长上下文和工具调用是否一致 |
| **可用率** | 一段时间内请求成功的比例 | 伪 200（返回 200 但内容为空或报错）、流式中途断开、格式错误都应算失败；样本量太小的「100%」参考价值很低 |
| **价格倍率** | 站点报价相对官方价的折扣（如 0.15×） | 同一模型、同一币种下比较输入、输出、缓存读写单价；注意「1 倍汇率」与「真实美元计费」的差别；倍率低不等于可靠 |
| **退款政策** | 余额能否退、怎么退、要多久 | 页面写明「支持 / 有条件 / 需联系客服 / 未提供」的差别很大；没写清楚的，按不可退处理 |
| **小额试用** | 起充门槛和注册赠送额度 | 先用最低档或赠送额度跑一遍你自己的真实任务，确认扣费与日志一致后再加量 |

补充几条经验：

- **先定模型和客户端，再挑站。** Claude Code、Codex CLI、Cursor、Cline 对协议（Anthropic Messages / OpenAI Chat / Responses API）和流式细节要求不同，同一个站在不同客户端上表现可能完全不一样。
- **看分组，不只看站名。** 很多站一个模型挂多个分组（「满血」「特惠」「cheap」等），价格和质量按分组区分，评测记录也要对应到分组。
- **主用 + 备用。** 长期业务至少准备两家，配置好随时可切换。
- **看主体和联系方式。** 有无企业主体、能否开票、客服响应时间，决定了出问题时有没有人可找。

## 二、风险提醒

> [!WARNING]
> **不要大额充值。** 中转站行业门槛低、生命周期短，跑路、降智、限流、改价都很常见。按「一周用量」充值，用完再充。

- **留意跑路信号：** 突然大幅打折、限时充值返利、客服失联、提现 / 退款通道关闭、官网域名频繁更换、群公告频繁「维护」。
- **1 折、0.05× 这类价格要多问一句：** 上游从哪来？可能来自共享账号、盗刷卡或逆向接口，随时可能被封。Folkbench 博客收录过一篇用户投稿讲这个问题：[大模型中转站的灰产问题](https://folkbench.com/blog/llm-api-relay-gray-industry-user-submission?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)。
- **数据安全：** 你的 prompt 和代码会经过中转站服务器。涉及公司代码、客户数据时，优先选有企业主体、写明数据处理方式的站点，或者干脆走官方。
- **不要把 Key 贴到群里或 issue 里。** 报告问题时请打码。
- **「可用率 99%」看窗口和样本量。** 参考：[别再被 99% 可用率骗了：稳定性测试指南](https://folkbench.com/blog/ai-api-relay-stability-guide?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)。

## 三、如何自己检测一个中转站

不想完全依赖任何榜单，可以自己按下面顺序测一遍：

1. **连通性和协议：** 用你实际的客户端（不是网页聊天）发起流式请求、工具调用和一次失败重试，看是否完整返回。
2. **模型身份：** 用固定题库对照官方回答，关注推理题、长上下文定位题、代码修改题；同一题多次采样看一致率。
3. **计费核对：** 跑完一个小任务后，对照控制台日志里的输入 / 输出 / 缓存 token 数和扣费，确认与报价一致。
4. **稳定性：** 在你真正使用的时段持续请求一段时间，记录成功率、首字延迟和 p95 延迟。

可以参考的 Folkbench 页面：

| 页面 | 内容 |
|---|---|
| [评测方法与证据](https://folkbench.com/methodology?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 事实类型、测量窗口、稳定性公式（Wilson 区间、延迟变异系数）和发布规则 |
| [实时检测：用你自己的 Key 检测接口](https://folkbench.com/evaluations/api-quality?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 输入中转站地址和 Key，在线跑一次检测 |
| [降智检测方法说明](https://folkbench.com/blog/how-folkbench-detects-model-degradation?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 时事探针与模型指纹探针如何对照官方基线 |
| [模型真的「降智」了吗？](https://folkbench.com/blog/did-the-model-get-dumber?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 从一张翻车截图到一次对照实验 |
| [API 中转站可靠性检查](https://folkbench.com/blog/api-relay-reliability-checks?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 可靠性检查清单 |
| [写代码场景下的评测指南](https://folkbench.com/blog/ai-api-relay-software-engineering-evaluation-guide?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 编程场景怎么测 |
| [中转站哪个好？按使用场景选型](https://folkbench.com/zhongzhuanzhan-na-ge-hao?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 日常对话 / 编码 / 持续业务三类场景 |
| [Claude 中转站测评](https://folkbench.com/claude-zhongzhuanzhan-ceping?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) · [GPT / Codex 中转站测评](https://folkbench.com/gpt-codex-zhongzhuanzhan-ceping?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 按模型看价格与可用率 |
| [可用率榜](https://folkbench.com/rankings?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) · [稳定性榜](https://folkbench.com/rankings/stability?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) · [会话成本榜](https://folkbench.com/rankings/cost?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) · [数据安全榜](https://folkbench.com/rankings/data-security?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 各类榜单 |

## 四、分站数据（来自 Folkbench）

数据来源：[Folkbench 中转站列表](https://folkbench.com/stations?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)，抓取于 **2026-11-17（北京时间）**。可用率与延迟为近 24 小时窗口，页面每 15 分钟更新，下表只是当天的快照，最新数据请点进各站详情页。

> [!NOTE]
> 排序遵循 Folkbench 站点规则，包含合作等级和运营加权；部分数据为展示样例。「—」表示页面当时没有该项数据。数据安全等级的含义见 [数据安全榜](https://folkbench.com/rankings/data-security?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)。

| # | 中转站 | 覆盖模型 | 近 24h 平均可用率 | 首字延迟中位数 | API 倍率 | 数据安全等级 |
|---|---|---|---|---|---|---|
| 1 | [智流 FluxLane](https://folkbench.com/services/fluxlane?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 38 | 94.4% | 5.18 s | — | 4 级 |
| 2 | [IKunCode](https://folkbench.com/services/ikuncode?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 15 | 93.8% | 5.68 s | 0.15× | 0 级 |
| 3 | [Cloudbridge](https://folkbench.com/services/cloudbridge?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 10 | 93.8% | 7.31 s | — | 0 级 |
| 4 | [CheapAI](https://folkbench.com/services/cheapai?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 11 | — | — | 0.098× | 3 级 |
| 5 | [A6api](https://folkbench.com/services/a6api?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 23 | — | — | — | 0 级 |
| 6 | [aicentos](https://folkbench.com/services/aicentos?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 30 | — | — | 0.21× | 0 级 |
| 7 | [超稳API / Stable API](https://folkbench.com/services/apiniuma?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | — | — | — | — | 0 级 |
| 8 | [Apiporter](https://folkbench.com/services/apiporter?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 6 | — | — | — | 0 级 |
| 9 | [aruhub API](https://folkbench.com/services/aruhub?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 18 | — | — | 0.13× | 0 级 |
| 10 | [CheapRouter](https://folkbench.com/services/cheaprouter?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 5 | — | — | — | 0 级 |
| 11 | [金石API](https://folkbench.com/services/merchant-2047c972-456?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 24 | — | — | 0.15× | 0 级 |
| 12 | [序点算力](https://folkbench.com/services/merchant-2e16cda8-294?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | — | — | — | — | 0 级 |
| 13 | [飞梁API](https://folkbench.com/services/merchant-316d7f43-b18?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 4 | — | — | — | 0 级 |
| 14 | [Zashmodel](https://folkbench.com/services/merchant-5cf7678f-611?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | — | — | — | — | 0 级 |
| 15 | [FlyRouter](https://folkbench.com/services/merchant-618ddad0-3b3?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 18 | — | — | 0.25× | 0 级 |
| 16 | [Buytoken](https://folkbench.com/services/merchant-72e9eb4a-5e0?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | — | — | — | — | 0 级 |
| 17 | [久连 9Link](https://folkbench.com/services/merchant-7a09254e-f3f?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 3 | — | — | 0.089× | 0 级 |
| 18 | [yukinahub](https://folkbench.com/services/merchant-80a0ee87-bc3?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | — | — | — | — | 0 级 |
| 19 | [Sidrune AI](https://folkbench.com/services/merchant-955eb49b-34a?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | — | — | — | — | 0 级 |
| 20 | [兽浮云中转站](https://folkbench.com/services/shoufuyun?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 1 | — | — | — | 0 级 |
| 21 | [SU8 Codes](https://folkbench.com/services/merchant-9adafd15-54e?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | — | — | — | — | 0 级 |
| 22 | [简渡API](https://folkbench.com/services/merchant-cd814303-0b7?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | — | — | — | — | 0 级 |
| 23 | [XINGYI API](https://folkbench.com/services/merchant-deafb24c-c5f?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | — | — | — | — | 0 级 |
| 24 | [ZNB.ai](https://folkbench.com/services/znb-ai?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 21 | — | — | 0.20× | 0 级 |

### 分站资料卡

> 以下字段摘自各站在 Folkbench 详情页上展示的「服务信息」，多为站长提交或页面观察，标注「未核实 / 未提供」的项请自行向站点确认。没有列出的字段表示页面暂未展示。

#### 1. 智流 FluxLane

- Folkbench 详情页：<https://folkbench.com/services/fluxlane?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 38，近 24 小时平均可用率 94.4%，首字延迟中位数 5.18 s，API 倍率 —，数据安全 4 级
- 服务信息：充值门槛：10 CNY；注册赠送：注册赠送2美元；客服响应：工作日30分钟内；支付方式：支付宝、微信支付；国内发票：支持；退款：支持

#### 2. IKunCode

- Folkbench 详情页：<https://folkbench.com/services/ikuncode?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 15，近 24 小时平均可用率 93.8%，首字延迟中位数 5.68 s，API 倍率 0.15×，数据安全 0 级
- 服务信息：充值门槛：5 CNY；注册赠送：0；客服响应：开票请提前联系客服；支付方式：支付宝、微信支付；国内发票：支持；退款：支持
- 页面列出的优点：有完善的售后群，客服响应快
- 页面列出的不足：扩展更多的定价分组

#### 3. Cloudbridge

- Folkbench 详情页：<https://folkbench.com/services/cloudbridge?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 10，近 24 小时平均可用率 93.8%，首字延迟中位数 7.31 s，API 倍率 —，数据安全 0 级
- 服务信息：充值门槛：1 USD；注册赠送：0.2美元；客服响应：工作日1小时内；支付方式：支付宝、微信支付、银行卡、PayPal、其他；国内发票：不支持；退款：需联系客服
- 页面列出的优点：在智谱、DeepSeek 等国产模型上具备价格优势
- 页面列出的不足：提高模型可用率和响应速度

#### 4. CheapAI

- Folkbench 详情页：<https://folkbench.com/services/cheapai?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 11，近 24 小时暂无成功请求记录，API 倍率 0.098×，数据安全 3 级
- 服务信息：充值门槛：20 USD；注册赠送：无；客服响应：工作日 4 小时内；支付方式：支付宝、微信支付、银行卡、其他；国内发票：支持；退款：有条件
- 页面列出的优点：价格极低，主打源头中转：官方宣传 0.08 倍费率、1 倍汇率，定位“GPT 源头中转，不限并发”。
- 页面列出的不足：长上下文场景的可靠性需要加强

#### 5. A6api

- Folkbench 详情页：<https://folkbench.com/services/a6api?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 23，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：未提供；客服响应：无；支付方式：支付宝、微信支付、银行卡、USDT、其他；国内发票：未核实；退款：未核实

#### 6. aicentos

- Folkbench 详情页：<https://folkbench.com/services/aicentos?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 30，近 24 小时暂无成功请求记录，API 倍率 0.21×，数据安全 0 级
- 服务信息：充值门槛：未提供；注册赠送：¥1；客服响应：未提供；支付方式：支付宝、微信支付、其他；国内发票：支持；退款：支持

#### 7. 超稳API / Stable API

- Folkbench 详情页：<https://folkbench.com/services/apiniuma?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 —，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级
- 服务信息：退款：尚未提供

#### 8. Apiporter

- Folkbench 详情页：<https://folkbench.com/services/apiporter?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 6，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级
- 服务信息：充值门槛：¥5；注册赠送：¥0.4；客服响应：工作日内1小时；支付方式：支付宝、其他；国内发票：支持；退款：需联系客服
- 页面列出的优点：起充较低，并且支持任意金额可开票
- 页面列出的不足：增强claude模型的稳定性，减少443出现

#### 9. aruhub API

- Folkbench 详情页：<https://folkbench.com/services/aruhub?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 18，近 24 小时暂无成功请求记录，API 倍率 0.13×，数据安全 0 级
- 服务信息：充值门槛：未提供；注册赠送：未提供；客服响应：未提供；支付方式：其他；国内发票：未核实；退款：未核实

#### 10. CheapRouter

- Folkbench 详情页：<https://folkbench.com/services/cheaprouter?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 5，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：未提供；客服响应：工作日4小时内；支付方式：支付宝、微信支付、银行卡、USDT、其他；国内发票：支持；退款：有条件

#### 11. 金石API

- Folkbench 详情页：<https://folkbench.com/services/merchant-2047c972-456?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 24，近 24 小时暂无成功请求记录，API 倍率 0.15×，数据安全 0 级
- 服务信息：充值门槛：¥1；注册赠送：未提供；客服响应：未提供；支付方式：支付宝、微信支付；国内发票：有条件；退款：未核实

#### 12. 序点算力

- Folkbench 详情页：<https://folkbench.com/services/merchant-2e16cda8-294?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 —，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级
- 服务信息：退款：不支持

#### 13. 飞梁API

- Folkbench 详情页：<https://folkbench.com/services/merchant-316d7f43-b18?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 4，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级
- 服务信息：退款：条件支持

#### 14. Zashmodel

- Folkbench 详情页：<https://folkbench.com/services/merchant-5cf7678f-611?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 —，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级
- 服务信息：退款：尚未提供

#### 15. FlyRouter

- Folkbench 详情页：<https://folkbench.com/services/merchant-618ddad0-3b3?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 18，近 24 小时暂无成功请求记录，API 倍率 0.25×，数据安全 0 级
- 服务信息：充值门槛：$1；注册赠送：$1；客服响应：半小时内；支付方式：银行卡、微信支付、支付宝；国内发票：有条件；退款：有条件

#### 16. Buytoken

- Folkbench 详情页：<https://folkbench.com/services/merchant-72e9eb4a-5e0?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 —，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级
- 服务信息：退款：支持

#### 17. 久连 9Link

- Folkbench 详情页：<https://folkbench.com/services/merchant-7a09254e-f3f?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 3，近 24 小时暂无成功请求记录，API 倍率 0.089×，数据安全 0 级
- 服务信息：充值门槛：未提供；注册赠送：未提供；客服响应：未提供；支付方式：微信支付、支付宝；国内发票：支持；退款：支持

#### 18. yukinahub

- Folkbench 详情页：<https://folkbench.com/services/merchant-80a0ee87-bc3?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 —，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级
- 服务信息：退款：尚未提供

#### 19. Sidrune AI

- Folkbench 详情页：<https://folkbench.com/services/merchant-955eb49b-34a?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 —，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级
- 服务信息：退款：支持

#### 20. 兽浮云中转站

- Folkbench 详情页：<https://folkbench.com/services/shoufuyun?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 1，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：未提供；客服响应：全天；支付方式：微信支付、支付宝；国内发票：不适用；退款：支持

#### 21. SU8 Codes

- Folkbench 详情页：<https://folkbench.com/services/merchant-9adafd15-54e?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 —，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级

#### 22. 简渡API

- Folkbench 详情页：<https://folkbench.com/services/merchant-cd814303-0b7?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 —，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级

#### 23. XINGYI API

- Folkbench 详情页：<https://folkbench.com/services/merchant-deafb24c-c5f?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 —，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级

#### 24. ZNB.ai

- Folkbench 详情页：<https://folkbench.com/services/znb-ai?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 21，近 24 小时暂无成功请求记录，API 倍率 0.20×，数据安全 0 级
- 服务信息：充值门槛：未提供；注册赠送：未提供；客服响应：未提供；支付方式：未提供；国内发票：未提供；退款：未提供

## 五、需谨慎

截至 2026-11-17，Folkbench 没有公开「不推荐 / 需谨慎」名单，所以这里**暂不点名任何站点**。我们会按下面的标准收集线索，核实后再写进来：

- 已确认的跑路、长期无法登录或无法提现 / 退款；
- 有可复现证据的模型替换或降级（同一题目、同一分组、带时间戳）；
- 计费与日志明显不符，且站点拒绝说明；
- 泄露用户 Key 或数据的安全事件。

遇到这类问题，请用 [踩坑报告](../../issues/new?template=pitfall-report.yml) 提交，附上时间、模型、分组和打码后的截图。对某个站点的数据有异议，也可以走 Folkbench 的 [纠错与申诉](https://folkbench.com/corrections?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)。

## 六、Folkbench Switch 桌面端

[Folkbench Switch](https://folkbench.com/switch?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) 是一个 AI 编程工具配置与切换的桌面应用，用来在多个中转站 / Key 之间切换 Claude Code、Codex 等工具的配置，方便做「主用 + 备用」。

- 当前版本：**0.1.0-beta.8**（预发布版，2026-11-16 发布）
- 下载：<https://github.com/Folkbench/folkbench/releases>
- 源码与反馈：<https://github.com/Folkbench/folkbench>

## 七、FAQ

**Q：哪家中转站最好？**
没有对所有模型和场景都最好的一家。先确定模型、客户端和预算，再看对应模型的记录，最后用小额试用验证。

**Q：这个列表的顺序是怎么来的？**
与 Folkbench 中转站列表页一致。排序遵循 Folkbench 站点规则，包含合作等级和运营加权；部分数据为展示样例。

**Q：为什么很多站点的可用率是「—」？**
表示近 24 小时 Folkbench 没有记录到该站的成功请求，或者还没有接入采样。没有记录不代表不可用，也不代表可用，需要你自己测。

**Q：价格倍率越低越好吗？**
不是。倍率低可能意味着上游来源不稳定或分组质量不同。请结合可用率、退款政策和自己的测试结果判断。

**Q：可以只看注册赠送额度来测吗？**
可以先用赠送额度测连通性，但请务必再用一个真实任务核对扣费和模型表现。

**Q：我是站长，怎么被收录？**
提交 [申请收录 issue](../../issues/new?template=site-submission.yml)，或者直接去 [Folkbench 站长入驻](https://folkbench.com/vendor-join?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)。本仓库的分站数据跟随 Folkbench 页面更新。

**Q：发现数据错了怎么办？**
提 issue，或走 Folkbench [纠错与申诉](https://folkbench.com/corrections?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)。

## 八、申请收录 / 踩坑报告

- 站长申请收录：[新建「申请收录」issue](../../issues/new?template=site-submission.yml)
- 用户踩坑 / 问题反馈：[新建「问题反馈 / 踩坑报告」issue](../../issues/new?template=pitfall-report.yml)
- 想改进本指南：见 [CONTRIBUTING.md](CONTRIBUTING.md)

## 更新日志

- **2026-11-17**：仓库创建。加入选站标准、风险提醒、自测方法、Folkbench 分站数据快照（24 家）、需谨慎收录标准、Folkbench Switch 0.1.0-beta.8 说明和 FAQ。

---

如果这份指南帮你避了坑，点个 ⭐ Star，方便下次找到，也让更多人看到。
