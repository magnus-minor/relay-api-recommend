# AI API 中转站推荐与选站指南（Claude / GPT / Gemini API中转）

> **English:** A Chinese-language guide to choosing an AI API relay (中转站 / relay api / api proxy) for Claude, GPT and Gemini: selection checklist, risk warnings, how to self-check a relay for model downgrades, and per-site data cards linked to [Folkbench](https://folkbench.com/?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend). Issues for new site submissions and pitfall reports are welcome.

> [!IMPORTANT]
> **披露：排序遵循 Folkbench 站点规则，包含合作等级和运营加权；部分数据为展示样例。**
> 本仓库的分站列表顺序与 Folkbench 中转站列表页一致，数据以 Folkbench 页面当前显示为准。任何列表都不能替代你自己的小额试用。

**目录**

- [一、中转站选站标准](#一中转站选站标准)
- [二、中转站充值风险提醒](#二中转站充值风险提醒)
- [三、如何自己检测一个中转站（降智 / 掺假）](#三如何自己检测一个中转站降智--掺假)
- [四、中转站推荐列表与分站数据（来自 Folkbench）](#四中转站推荐列表与分站数据来自-folkbench)
- [五、需谨慎的中转站](#五需谨慎的中转站)
- [六、Folkbench Switch 桌面端](#六folkbench-switch-桌面端)
- [七、中转站常见问题 FAQ](#七中转站常见问题-faq)
- [八、中转站申请收录 / 踩坑报告](#八中转站申请收录--踩坑报告)
- [九、相关链接：中转站公开快照子站](#九相关链接中转站公开快照子站)
- [十、English quick guide](#十english-quick-guide)
- [更新日志](#更新日志)

---

## 一、中转站选站标准

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

## 二、中转站充值风险提醒

> [!WARNING]
> **不要大额充值。** 中转站行业门槛低、生命周期短，跑路、降智、限流、改价都很常见。按「一周用量」充值，用完再充。

- **留意跑路信号：** 突然大幅打折、限时充值返利、客服失联、提现 / 退款通道关闭、官网域名频繁更换、群公告频繁「维护」。
- **1 折、0.05× 这类价格要多问一句：** 上游从哪来？可能来自共享账号、盗刷卡或逆向接口，随时可能被封。Folkbench 博客收录过一篇用户投稿讲这个问题：[大模型中转站的灰产问题](https://folkbench.com/blog/llm-api-relay-gray-industry-user-submission?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)。
- **数据安全：** 你的 prompt 和代码会经过中转站服务器。涉及公司代码、客户数据时，优先选有企业主体、写明数据处理方式的站点，或者干脆走官方。
- **不要把 Key 贴到群里或 issue 里。** 报告问题时请打码。
- **「可用率 99%」看窗口和样本量。** 参考：[别再被 99% 可用率骗了：稳定性测试指南](https://folkbench.com/blog/ai-api-relay-stability-guide?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)。

## 三、如何自己检测一个中转站（降智 / 掺假）

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

## 四、中转站推荐列表与分站数据（来自 Folkbench）

数据来源：[Folkbench 中转站列表](https://folkbench.com/stations?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)，抓取于 **2026-11-22 10:50（北京时间）**。可用率与延迟为近 24 小时窗口，页面每 15 分钟更新，下表只是当天的快照，最新数据请点进各站详情页。

> [!NOTE]
> 排序遵循 Folkbench 站点规则，包含合作等级和运营加权；部分数据为展示样例。「—」表示页面当时没有该项数据。数据安全等级的含义见 [数据安全榜](https://folkbench.com/rankings/data-security?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)。

| # | 中转站 | 覆盖模型 | 近 24h 平均可用率 | 首字延迟中位数 | API 倍率 | 数据安全等级 |
|---|---|---|---|---|---|---|
| 1 | [智流 FluxLane](https://folkbench.com/services/fluxlane?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 47 | 100.0% | 5.73 s | 0.12× | 2 级 |
| 2 | [Apiporter](https://folkbench.com/services/apiporter?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 12 | 100.0% | 2.62 s | 0.30× | 0 级 |
| 3 | [Cloudbridge](https://folkbench.com/services/cloudbridge?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 11 | 100.0% | 2.77 s | 0.23× | 0 级 |
| 4 | [IKunCode](https://folkbench.com/services/ikuncode?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 35 | 100.0% | 3.56 s | 0.15× | 0 级 |
| 5 | [ModelPod](https://folkbench.com/services/merchant-27af1806-16e?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 25 | 100.0% | 1.84 s | 0.15× | 0 级 |
| 6 | [破晓](https://folkbench.com/services/merchant-4cf2b788-920?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 25 | 100.0% | 2.60 s | 0.030× | 0 级 |
| 7 | [Zashmodel](https://folkbench.com/services/merchant-5cf7678f-611?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 36 | 100.0% | 3.49 s | 0.045× | 0 级 |
| 8 | [Sidrune AI](https://folkbench.com/services/merchant-955eb49b-34a?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 47 | 100.0% | 3.77 s | 0.18× | 0 级 |
| 9 | [COFFEE API](https://folkbench.com/services/merchant-dbbb1c95-135?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 8 | 100.0% | 3.55 s | 0.030× | 0 级 |
| 10 | [ZNB.ai](https://folkbench.com/services/znb-ai?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 28 | 100.0% | 3.21 s | 0.056× | 0 级 |
| 11 | [aicode](https://folkbench.com/services/merchant-2b1bbef9-885?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 4 | 96.9% | 3.64 s | 0.038× | 0 级 |
| 12 | [CheapAI](https://folkbench.com/services/cheapai?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 48 | — | — | 0.045× | 3 级 |
| 13 | [A6api](https://folkbench.com/services/a6api?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 67 | — | — | 0.049× | 0 级 |
| 14 | [aicentos](https://folkbench.com/services/aicentos?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 26 | — | — | 0.22× | 0 级 |
| 15 | [超稳API / Stable API](https://folkbench.com/services/apiniuma?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 65 | — | — | 0.15× | 0 级 |
| 16 | [aruhub API](https://folkbench.com/services/aruhub?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 37 | — | — | 0.089× | 0 级 |
| 17 | [CheapRouter](https://folkbench.com/services/cheaprouter?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 28 | — | — | 0.074× | 0 级 |
| 18 | [金石API](https://folkbench.com/services/merchant-2047c972-456?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 34 | — | — | 0.12× | 0 级 |
| 19 | [序点算力](https://folkbench.com/services/merchant-2e16cda8-294?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 25 | — | — | 0.045× | 0 级 |
| 20 | [飞梁API](https://folkbench.com/services/merchant-316d7f43-b18?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 38 | — | — | 0.071× | 0 级 |
| 21 | [HardysAPI](https://folkbench.com/services/merchant-3e458582-c5c?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 64 | — | — | 0.14× | 0 级 |
| 22 | [AIGROW](https://folkbench.com/services/merchant-5be36043-bc1?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 9 | — | — | — | 0 级 |
| 23 | [FlyRouter](https://folkbench.com/services/merchant-618ddad0-3b3?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 31 | — | — | 0.25× | 0 级 |
| 24 | [Buytoken](https://folkbench.com/services/merchant-72e9eb4a-5e0?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 24 | — | — | 0.097× | 0 级 |
| 25 | [久连 9Link](https://folkbench.com/services/merchant-7a09254e-f3f?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 6 | — | — | 0.089× | 0 级 |
| 26 | [yukinahub](https://folkbench.com/services/merchant-80a0ee87-bc3?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 38 | — | — | 0.037× | 0 级 |
| 27 | [SU8 Codes](https://folkbench.com/services/merchant-9adafd15-54e?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | — | — | — | — | 0 级 |
| 28 | [TokenMilo](https://folkbench.com/services/merchant-bf7e2fcb-e08?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 15 | — | — | 0.21× | 0 级 |
| 29 | [简渡API](https://folkbench.com/services/merchant-cd814303-0b7?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | — | — | — | — | 0 级 |
| 30 | [GQuick AI](https://folkbench.com/services/merchant-d1c1e380-bb9?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 12 | — | — | 0.097× | 0 级 |
| 31 | [XINGYI API](https://folkbench.com/services/merchant-deafb24c-c5f?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | — | — | — | — | 0 级 |
| 32 | [兽浮云中转站](https://folkbench.com/services/shoufuyun?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) | 2 | — | — | 0.015× | 0 级 |

### 分站资料卡

> 以下字段摘自各站在 Folkbench 详情页上展示的「服务信息」，多为站长提交或页面观察，标注「未核实 / 未提供」的项请自行向站点确认。没有列出的字段表示页面暂未展示。

#### 1. 智流 FluxLane

- Folkbench 详情页：<https://folkbench.com/services/fluxlane?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 47，近 24 小时平均可用率 100.0%，首字延迟中位数 5.73 s，API 倍率 0.12×，数据安全 2 级
- 服务信息：充值门槛：10 CNY；注册赠送：注册赠送2美元；客服响应：工作日30分钟内；支付方式：支付宝、微信支付；国内发票：支持；退款：支持

#### 2. Apiporter

- Folkbench 详情页：<https://folkbench.com/services/apiporter?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 12，近 24 小时平均可用率 100.0%，首字延迟中位数 2.62 s，API 倍率 0.30×，数据安全 0 级
- 服务信息：充值门槛：¥5；注册赠送：¥0.4；客服响应：工作日内1小时；支付方式：支付宝、其他；国内发票：支持；退款：需联系客服
- 页面列出的优点：起充较低，并且支持任意金额可开票
- 页面列出的不足：增强claude模型的稳定性，减少443出现

#### 3. Cloudbridge

- Folkbench 详情页：<https://folkbench.com/services/cloudbridge?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 11，近 24 小时平均可用率 100.0%，首字延迟中位数 2.77 s，API 倍率 0.23×，数据安全 0 级
- 服务信息：充值门槛：1 USD；注册赠送：0.2美元；客服响应：工作日1小时内；支付方式：支付宝、微信支付、银行卡、PayPal、其他；国内发票：不支持；退款：需联系客服
- 页面列出的优点：在智谱、DeepSeek 等国产模型上具备价格优势
- 页面列出的不足：提高模型可用率和响应速度

#### 4. IKunCode

- Folkbench 详情页：<https://folkbench.com/services/ikuncode?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 35，近 24 小时平均可用率 100.0%，首字延迟中位数 3.56 s，API 倍率 0.15×，数据安全 0 级
- 服务信息：充值门槛：5 CNY；注册赠送：0；客服响应：开票请提前联系客服；支付方式：支付宝、微信支付；国内发票：支持；退款：支持
- 页面列出的优点：有完善的售后群，客服响应快
- 页面列出的不足：扩展更多的定价分组

#### 5. ModelPod

- Folkbench 详情页：<https://folkbench.com/services/merchant-27af1806-16e?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 25，近 24 小时平均可用率 100.0%，首字延迟中位数 1.84 s，API 倍率 0.15×，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：¥0；客服响应：未提供；支付方式：支付宝、微信支付、银行卡；国内发票：有条件；退款：未核实

#### 6. 破晓

- Folkbench 详情页：<https://folkbench.com/services/merchant-4cf2b788-920?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 25，近 24 小时平均可用率 100.0%，首字延迟中位数 2.60 s，API 倍率 0.030×，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：未提供；客服响应：未提供；支付方式：支付宝、微信支付；国内发票：支持；退款：支持

#### 7. Zashmodel

- Folkbench 详情页：<https://folkbench.com/services/merchant-5cf7678f-611?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 36，近 24 小时平均可用率 100.0%，首字延迟中位数 3.49 s，API 倍率 0.045×，数据安全 0 级
- 服务信息：充值门槛：未提供；注册赠送：未提供；客服响应：未提供；支付方式：微信支付、银行卡、PayPal、其他；国内发票：未核实；退款：未核实

#### 8. Sidrune AI

- Folkbench 详情页：<https://folkbench.com/services/merchant-955eb49b-34a?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 47，近 24 小时平均可用率 100.0%，首字延迟中位数 3.77 s，API 倍率 0.18×，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：¥0；客服响应：未提供；支付方式：支付宝、微信支付、USDT；国内发票：支持；退款：支持

#### 9. COFFEE API

- Folkbench 详情页：<https://folkbench.com/services/merchant-dbbb1c95-135?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 8，近 24 小时平均可用率 100.0%，首字延迟中位数 3.55 s，API 倍率 0.030×，数据安全 0 级
- 服务信息：充值门槛：未提供；注册赠送：未提供；客服响应：未提供；支付方式：微信支付、支付宝；国内发票：未核实；退款：未核实

#### 10. ZNB.ai

- Folkbench 详情页：<https://folkbench.com/services/znb-ai?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 28，近 24 小时平均可用率 100.0%，首字延迟中位数 3.21 s，API 倍率 0.056×，数据安全 0 级
- 服务信息：充值门槛：未提供；注册赠送：未提供；客服响应：未提供；支付方式：未提供；国内发票：未提供；退款：未提供

#### 11. aicode

- Folkbench 详情页：<https://folkbench.com/services/merchant-2b1bbef9-885?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 4，近 24 小时平均可用率 96.9%，首字延迟中位数 3.64 s，API 倍率 0.038×，数据安全 0 级
- 服务信息：充值门槛：未提供；注册赠送：未提供；客服响应：未提供；支付方式：微信支付、支付宝；国内发票：未核实；退款：未核实

#### 12. CheapAI

- Folkbench 详情页：<https://folkbench.com/services/cheapai?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 48，近 24 小时暂无成功请求记录，API 倍率 0.045×，数据安全 3 级
- 服务信息：充值门槛：20 USD；注册赠送：无；客服响应：工作日 4 小时内；支付方式：支付宝、微信支付、银行卡、其他；国内发票：支持；退款：有条件
- 页面列出的优点：价格极低，主打源头中转：官方宣传 0.08 倍费率、1 倍汇率，定位“GPT 源头中转，不限并发”。
- 页面列出的不足：长上下文场景的可靠性需要加强

#### 13. A6api

- Folkbench 详情页：<https://folkbench.com/services/a6api?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 67，近 24 小时暂无成功请求记录，API 倍率 0.049×，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：未提供；客服响应：无；支付方式：支付宝、微信支付、银行卡、USDT、其他；国内发票：未核实；退款：未核实

#### 14. aicentos

- Folkbench 详情页：<https://folkbench.com/services/aicentos?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 26，近 24 小时暂无成功请求记录，API 倍率 0.22×，数据安全 0 级
- 服务信息：充值门槛：未提供；注册赠送：¥1；客服响应：未提供；支付方式：支付宝、微信支付、其他；国内发票：支持；退款：支持

#### 15. 超稳API / Stable API

- Folkbench 详情页：<https://folkbench.com/services/apiniuma?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 65，近 24 小时暂无成功请求记录，API 倍率 0.15×，数据安全 0 级
- 服务信息：充值门槛：$1；注册赠送：未提供；客服响应：新加坡时间周一至周五 10:00-17:30（以官网公示客服时间为准）；支付方式：支付宝、微信支付、其他；国内发票：未核实；退款：未核实

#### 16. aruhub API

- Folkbench 详情页：<https://folkbench.com/services/aruhub?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 37，近 24 小时暂无成功请求记录，API 倍率 0.089×，数据安全 0 级
- 服务信息：充值门槛：未提供；注册赠送：未提供；客服响应：未提供；支付方式：其他；国内发票：未核实；退款：未核实

#### 17. CheapRouter

- Folkbench 详情页：<https://folkbench.com/services/cheaprouter?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 28，近 24 小时暂无成功请求记录，API 倍率 0.074×，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：未提供；客服响应：工作日4小时内；支付方式：支付宝、微信支付、银行卡、USDT、其他；国内发票：支持；退款：有条件

#### 18. 金石API

- Folkbench 详情页：<https://folkbench.com/services/merchant-2047c972-456?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 34，近 24 小时暂无成功请求记录，API 倍率 0.12×，数据安全 0 级
- 服务信息：充值门槛：¥1；注册赠送：未提供；客服响应：未提供；支付方式：支付宝、微信支付；国内发票：有条件；退款：未核实

#### 19. 序点算力

- Folkbench 详情页：<https://folkbench.com/services/merchant-2e16cda8-294?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 25，近 24 小时暂无成功请求记录，API 倍率 0.045×，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：未提供；客服响应：无；支付方式：微信支付；国内发票：支持；退款：不支持

#### 20. 飞梁API

- Folkbench 详情页：<https://folkbench.com/services/merchant-316d7f43-b18?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 38，近 24 小时暂无成功请求记录，API 倍率 0.071×，数据安全 0 级
- 服务信息：充值门槛：¥1；注册赠送：¥1；客服响应：8:00-23:30；支付方式：支付宝、微信支付；国内发票：不支持；退款：有条件

#### 21. HardysAPI

- Folkbench 详情页：<https://folkbench.com/services/merchant-3e458582-c5c?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 64，近 24 小时暂无成功请求记录，API 倍率 0.14×，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：¥0.1；客服响应：8小时内；支付方式：微信支付、支付宝、USDT、其他；国内发票：不适用；退款：不支持

#### 22. AIGROW

- Folkbench 详情页：<https://folkbench.com/services/merchant-5be36043-bc1?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 9，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级
- 服务信息：退款：条件支持

#### 23. FlyRouter

- Folkbench 详情页：<https://folkbench.com/services/merchant-618ddad0-3b3?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 31，近 24 小时暂无成功请求记录，API 倍率 0.25×，数据安全 0 级
- 服务信息：充值门槛：$1；注册赠送：$1；客服响应：半小时内；支付方式：银行卡、微信支付、支付宝；国内发票：有条件；退款：有条件

#### 24. Buytoken

- Folkbench 详情页：<https://folkbench.com/services/merchant-72e9eb4a-5e0?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 24，近 24 小时暂无成功请求记录，API 倍率 0.097×，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：未提供；客服响应：无；支付方式：支付宝、微信支付、银行卡；国内发票：支持；退款：支持

#### 25. 久连 9Link

- Folkbench 详情页：<https://folkbench.com/services/merchant-7a09254e-f3f?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 6，近 24 小时暂无成功请求记录，API 倍率 0.089×，数据安全 0 级
- 服务信息：充值门槛：未提供；注册赠送：未提供；客服响应：未提供；支付方式：微信支付、支付宝；国内发票：支持；退款：支持

#### 26. yukinahub

- Folkbench 详情页：<https://folkbench.com/services/merchant-80a0ee87-bc3?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 38，近 24 小时暂无成功请求记录，API 倍率 0.037×，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：未提供；客服响应：无；支付方式：支付宝、微信支付、USDT；国内发票：未核实；退款：未核实

#### 27. SU8 Codes

- Folkbench 详情页：<https://folkbench.com/services/merchant-9adafd15-54e?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 —，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级

#### 28. TokenMilo

- Folkbench 详情页：<https://folkbench.com/services/merchant-bf7e2fcb-e08?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 15，近 24 小时暂无成功请求记录，API 倍率 0.21×，数据安全 0 级
- 服务信息：充值门槛：¥50；注册赠送：$5；客服响应：北京时间 9:00–22:00 在线；支付方式：微信支付；国内发票：不支持；退款：有条件

#### 29. 简渡API

- Folkbench 详情页：<https://folkbench.com/services/merchant-cd814303-0b7?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 —，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级

#### 30. GQuick AI

- Folkbench 详情页：<https://folkbench.com/services/merchant-d1c1e380-bb9?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 12，近 24 小时暂无成功请求记录，API 倍率 0.097×，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：¥0；客服响应：1小时内；支付方式：微信支付、支付宝、其他；国内发票：未核实；退款：未核实

#### 31. XINGYI API

- Folkbench 详情页：<https://folkbench.com/services/merchant-deafb24c-c5f?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 —，近 24 小时暂无成功请求记录，API 倍率 —，数据安全 0 级

#### 32. 兽浮云中转站

- Folkbench 详情页：<https://folkbench.com/services/shoufuyun?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- 列表页数据：覆盖模型 2，近 24 小时暂无成功请求记录，API 倍率 0.015×，数据安全 0 级
- 服务信息：充值门槛：¥10；注册赠送：未提供；客服响应：全天；支付方式：微信支付、支付宝；国内发票：不适用；退款：支持


### 按模型的 Claude / GPT Codex 测评快照

数据来源：Folkbench [Claude 中转站测评](https://folkbench.com/claude-zhongzhuanzhan-ceping?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) 与 [GPT / Codex 中转站测评](https://folkbench.com/gpt-codex-zhongzhuanzhan-ceping?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)，抓取于 **2026-11-21（北京时间）**，每周更新一次。价格为每百万 token 的输入 / 输出报价；可用率和样本数来自页面标注的采样时段，「—」表示该分组在采样时段内没有已发布记录。

> [!NOTE]
> 排序遵循 Folkbench 站点规则，包含合作等级和运营加权；部分数据为展示样例。每个模型最多摘录 5 条，完整列表与最新数据以测评页为准。想测自己手上的 Key，可用 [实时检测页](https://folkbench.com/evaluations/api-quality?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)。

**Claude Opus 5.5**（Claude，数据发布于 2026年11月21日）

| 中转站 | 分组 | 可用率 | 输入 | 输出 | 样本数 |
|---|---|---|---|---|---|
| IKunCode | cc-kiro | 100.00% | ¥4.00 | ¥20.00 | 48 |
| Apiporter | Claude_core | — | ¥8.00 | ¥40.00 | — |
| 智流 FluxLane | Claude-Max｜满血旗舰 | 97.92% | ¥4.80 | ¥24.00 | 48 |
| ZNB.ai | znb-cc-特惠 | — | ¥0.60 | ¥3.00 | — |
| Cloudbridge | Claude-core | 95.83% | ¥12.48 | ¥62.40 | 48 |

**Claude Fable 5.1**（Claude，数据发布于 2026年11月21日）

| 中转站 | 分组 | 可用率 | 输入 | 输出 | 样本数 |
|---|---|---|---|---|---|
| Apiporter | Claude_core | 100.00% | ¥20.00 | ¥100.00 | 3 |
| ZNB.ai | znb-cc-特惠 | — | ¥1.50 | ¥7.50 | — |
| Cloudbridge | Claude-core | 97.92% | ¥31.20 | ¥156.00 | 48 |
| 88API | Claude Max池 可外接 | — | ¥14.00 | ¥70.00 | — |

**Claude Opus 5**（Claude，数据发布于 2026年11月21日）

| 中转站 | 分组 | 可用率 | 输入 | 输出 | 样本数 |
|---|---|---|---|---|---|
| IKunCode | cc逆向 | — | ¥3.50 | ¥17.50 | — |
| 智流 FluxLane | Claude-Kiro｜增强优选 | — | ¥2.00 | ¥10.00 | — |
| Sidrune AI | Claude-Kiro | — | ¥1.25 | ¥6.25 | — |
| 破晓 | kiro | — | ¥1.00 | ¥5.00 | — |
| ZNB.ai | znb-kiro-稳定 | — | ¥2.00 | ¥10.00 | — |

**GPT 6.1 Sol**（GPT / Codex，数据发布于 2026年11月21日）

| 中转站 | 分组 | 可用率 | 输入 | 输出 | 样本数 |
|---|---|---|---|---|---|
| Sidrune AI | GPT-限时福利 | 95.83% | ¥0.20 | ¥1.00 | 48 |
| IKunCode | Codex | — | ¥0.50 | ¥2.50 | — |
| Zashmodel | gpt-prime | 95.83% | — | — | 48 |
| Apiporter | Codex_core | — | ¥0.54 | ¥2.70 | — |
| ZNB.ai | znb-gpt-特惠 | 97.92% | ¥0.12 | ¥0.60 | 48 |

**GPT-6.2 Sol**（GPT / Codex，数据发布于 2026年11月21日）

| 中转站 | 分组 | 可用率 | 输入 | 输出 | 样本数 |
|---|---|---|---|---|---|
| Apiporter | Codex_core | — | ¥20.25 | ¥40.50 | — |
| 智流 FluxLane | GPT-PLUS｜优选线路 | — | ¥0.20 | ¥1.00 | — |
| aicode | 028 OpenAI 不降智 | 100.00% | ¥0.56 | ¥2.80 | 31 |
| Sidrune AI | GPT-Azure | — | ¥5.00 | ¥25.00 | — |
| 破晓 | 【限时】Codex 双十一 不降智 | 100.00% | ¥0.10 | ¥0.50 | 31 |

**GPT-6.2 Astra**（GPT / Codex，数据发布于 2026年11月21日）

| 中转站 | 分组 | 可用率 | 输入 | 输出 | 样本数 |
|---|---|---|---|---|---|
| Apiporter | Codex_core | — | ¥20.25 | ¥40.50 | — |
| 智流 FluxLane | GPT-PLUS｜优选线路 | — | ¥1.00 | ¥5.00 | — |
| Sidrune AI | GPT-Azure | — | ¥20.00 | ¥100.00 | — |
| 破晓 | 【限时】Codex 双十一 不降智 | 100.00% | ¥0.40 | ¥2.00 | 31 |
| ZNB.ai | znb-gpt-特惠 | — | ¥0.48 | ¥2.40 | — |

## 五、需谨慎的中转站

截至 2026-11-17，Folkbench 没有公开「不推荐 / 需谨慎」名单，所以这里**暂不点名任何站点**。我们会按下面的标准收集线索，核实后再写进来：

- 已确认的跑路、长期无法登录或无法提现 / 退款；
- 有可复现证据的模型替换或降级（同一题目、同一分组、带时间戳）；
- 计费与日志明显不符，且站点拒绝说明；
- 泄露用户 Key 或数据的安全事件。

遇到这类问题，请用 [踩坑报告](../../issues/new?template=pitfall-report.yml) 提交，附上时间、模型、分组和打码后的截图。对某个站点的数据有异议，也可以走 Folkbench 的 [纠错与申诉](https://folkbench.com/corrections?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)。

## 六、Folkbench Switch 桌面端

[Folkbench Switch](https://folkbench.com/switch?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend) 是一个 AI 编程工具配置与切换的桌面应用，用来在多个中转站 / Key 之间切换 Claude Code、Codex 等工具的配置，方便做「主用 + 备用」。

- 当前版本：**0.1.0-beta.10**（预发布版，2026-11-19 发布）
- beta.10 更新要点：预设线路新增 Sidrune AI，简易模式、添加向导和专业模式均可直接选择；发现页站点详情的发票与退款信息改为本地化文字，并统一客户端设计令牌。
- 下载：<https://github.com/Folkbench/folkbench/releases>
- 源码与反馈：<https://github.com/Folkbench/folkbench>

## 七、中转站常见问题 FAQ

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

**Q：中转站充值后能退款吗？**
各站规则不同。先看上面分站资料卡里的「退款」字段（来自站长提交或页面展示，标注「未核实 / 未提供」时请向该站确认）。如果站点没有写明退款规则，先把余额按不能退处理。具体以该站最新说明和客服答复为准。

**Q：可以用自己的 Key 在 Folkbench 上检测吗？**
可以。打开 [实时检测页](https://folkbench.com/evaluations/api-quality?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)，用自己的 Key 检测；报告仅自己可见，密钥不保留。

**Q：数据安全等级（0 级 / 2 级 / 3 级）怎么看？**
等级来自 Folkbench [数据安全榜](https://folkbench.com/rankings/data-security?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)，榜单同时列出每家的「是否存在企业主体、国内支付、境外支付、发票、客服、退款」六项。2026-11-22 抓取时，CheapAI 为 3 级、智流 FluxLane 为 2 级，这两家企业主体一栏为「是」；榜单上的 32 家 0 级站点，企业主体一栏均为「否」。等级越低，页面上能核对的主体与售后信息越少，充值前建议照这六项向站点逐条确认，尤其是主体和退款。按 [评测方法](https://folkbench.com/methodology?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend)，有效核验的安全等级会计入派生分（每级加 2.5 分）。

## 八、中转站申请收录 / 踩坑报告

- 站长申请收录：[新建「申请收录」issue](../../issues/new?template=site-submission.yml)
- 用户踩坑 / 问题反馈：[新建「问题反馈 / 踩坑报告」issue](../../issues/new?template=pitfall-report.yml)
- 想改进本指南：见 [CONTRIBUTING.md](CONTRIBUTING.md)

## 九、相关链接：中转站公开快照子站

下面 6 个站点都由 Folkbench 公开快照每天生成，角度不同，可按需要查看。排序按站点规则，含合作档位与运营加权；部分数据为展示样本。

- [AI API 中转站公开快照（中转站排行）](https://folkbench.github.io/relay-rank/?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend&utm_content=relay-rank)：按模型查看各中转站的公开快照记录与站点档案。
- [中转站探针公开快照周报](https://folkbench.github.io/integrity-weekly/?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend&utm_content=integrity-weekly)：每周汇总已公开的模型身份探针结果。
- [AI 中转站模型对比](https://ai-relay-bench.github.io/?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend&utm_content=ai-relay-bench)：同一模型在各中转站的可用率、稳定性与公开价格对比。
- [AI API 公开价格快照](https://aiapi-rank.github.io/?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend&utm_content=aiapi-rank)：按模型汇总中转站公开标价，并按厂商列出相关站点。
- [AI 中转站公开目录](https://ai-zhongzhuan.github.io/?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend&utm_content=ai-zhongzhuan)：中转站目录、站点档案、支付与发票资料和新手指南。
- [模型身份探针公开快照](https://ai-model-check.github.io/?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend&utm_content=ai-model-check)：按模型和站点查看身份探针一致率与指纹观察；单次结果不能证明模型被替换。

## 十、English quick guide

A short English summary for readers searching for an **AI API relay**, **relay api** or **Claude / GPT API proxy** list.

**What a relay (中转站) is:** a third-party endpoint that resells access to Claude, GPT, Gemini and other models, usually at a fraction of the official price, behind an OpenAI-compatible or Anthropic-compatible API.

**Checklist before you top up**

1. **Model identity** – run the same fixed prompts against the relay and the official API; check reasoning depth, long-context recall and tool calls, not just "it replies".
2. **Availability** – count fake 200s (empty or error bodies), broken streams and malformed responses as failures; a "100%" on a tiny sample means little.
3. **Price multiplier** – compare input, output and cache read/write prices for the same model and currency; a low multiplier is not the same as reliable.
4. **Refunds** – if the refund policy is not written down, treat the balance as non-refundable.
5. **Small trial first** – use the minimum top-up or the sign-up credit on your real task, then check the token counts and charges in the console log.

**Client notes:** Claude Code needs the Anthropic Messages protocol; Codex CLI uses the OpenAI Responses API; Gemini CLI has its own protocol. The same relay can behave differently across clients, so test with the client you actually use.

**Useful Folkbench pages**

- Relay list with 24h availability, first-token latency and price multiplier: <https://folkbench.com/stations?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- Check a relay with your own key (results visible only to you, key not stored): <https://folkbench.com/evaluations/api-quality?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- Model downgrade monitor: <https://folkbench.com/arena?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- Rankings: <https://folkbench.com/rankings?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>
- Folkbench Switch, a desktop app to switch Claude Code / Codex / Gemini CLI between relays: <https://folkbench.com/switch?utm_source=github&utm_medium=readme&utm_campaign=relay-recommend>

Ordering on Folkbench and in this repo follows Folkbench site rules, including partnership tiers and operational weighting; some data are display samples.

## 更新日志

- **2026-11-22 10:50**：按 Folkbench 中转站列表页刷新分站数据快照（30→32 家，新增 COFFEE API、AIGROW）；前 10 家近 24h 可用率均为 100.0%，Cloudbridge 升到第 3、Zashmodel 升到第 7，aicode 可用率 96.9%、位次降到第 11；Apiporter、Cloudbridge、ModelPod、HardysAPI、aruhub API 等倍率有变化，多家覆盖模型数和延迟更新。FAQ 新增「数据安全等级怎么看」（按数据安全榜 2026-11-22 数据）。
- **2026-11-21 10:48**：按 Folkbench 中转站列表页刷新分站数据快照（29→30 家，新增 TokenMilo，即 #2 申请收录的站点）；Apiporter、ModelPod、aicode、破晓有了近 24h 可用率，位次随列表页调整；FluxLane、IKunCode、Sidrune AI、ZNB.ai、Zashmodel、Cloudbridge 的可用率和延迟更新，多家覆盖模型数和倍率有变化。新增「按模型的 Claude / GPT Codex 测评快照」（Claude Opus 5.5、Fable 5.1、Opus 5，GPT 6.1 Sol、6.2 Sol、6.2 Astra），每周更新。
- **2026-11-20 10:51**：按 Folkbench 中转站列表页刷新分站数据快照（26→29 家，新增 aicode、HardysAPI、破晓；ZNB.ai 有了近 24h 可用率 100.0%，位次升到第 3；FluxLane 可用率、延迟、倍率与数据安全等级更新，Sidrune AI、IKunCode、Zashmodel、Cloudbridge 的可用率和延迟有更新，多家覆盖模型数和倍率有变化）；FAQ 补充退款说明和自测入口；Folkbench Switch 更新到 0.1.0-beta.10。
- **2026-11-19 10:43**：按 Folkbench 中转站列表页刷新分站数据快照（25→26 家，新增 GQuick AI；Sidrune AI 有了近 24h 可用率 93.1%，位次升到第 3；FluxLane、IKunCode、Cloudbridge、Zashmodel 的可用率、延迟和倍率更新，多家覆盖模型数和倍率有变化）；新增「十、English quick guide」英文选站摘要；Folkbench Switch 更新到 0.1.0-beta.9 并补充更新要点；回复 #2（TokenMilo，引导先在 Folkbench 站长入驻）。

- **2026-11-18 10:43**：按 Folkbench 中转站列表页刷新分站数据快照（仍为 25 家，无增删）：智流 FluxLane、IKunCode、Cloudbridge、Zashmodel 的可用率和首字延迟更新，aicentos 覆盖模型 30→33，aruhub API 倍率 0.13×→0.20×；回复 #1（FlyRouter 已在列表中）。博客无新文章，「自测方法」链接表未改。
- **2026-11-17 23:30**：新增「相关链接」一节，列出 6 个中转站公开快照子站。
- **2026-11-17 22:45**：按 Folkbench 中转站列表页刷新分站数据快照（25 家，新增 ModelPod；Zashmodel 位次、可用率和延迟有更新）。
- **2026-11-17**：仓库创建。加入选站标准、风险提醒、自测方法、Folkbench 分站数据快照（24 家）、需谨慎收录标准、Folkbench Switch 0.1.0-beta.8 说明和 FAQ。

---

如果这份指南帮你避了坑，点个 ⭐ Star，方便下次找到，也让更多人看到。
