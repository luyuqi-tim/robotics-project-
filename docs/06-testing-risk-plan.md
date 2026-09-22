# Document 6 — Testing & Risk Plan

## 1. 测试金字塔

### Unit

- observation schema：shape、dtype、camera order、timestamp；
- action schema：维度、NaN/Inf、范围、norm tag；
- safety gate：关节限位、最大步长、最大速率、超时；
- metrics：成功率、分位数、回合完整性；
- config：缺失字段和非法组合拒绝启动。

### Integration

- 录制观测 → policy → action，不接机器人；
- 假 robot adapter → safety → executor；
- 相机断连、策略超时、串口断连故障注入；
- checkpoint revision 与 norm stats 匹配；
- ROS 节点与 direct pipeline 的 schema 一致性（Enhanced）。

### End-to-End

按顺序放权：

1. dry-run：只记录动作；
2. 电机上电但机械臂架空/无物；
3. 低速、单动作步；
4. 软质大物体、宽容器；
5. 完整任务；
6. 正式批量试验。

## 2. Baseline

- B0：固定输入离线推理稳定性；
- B1：人工遥操作/人工选择动作完成率，用于验证硬件任务本身可行；
- B2：MolmoAct2 零样本真机；
- B3（可选）：经过相机/动作配置优化后的同一 checkpoint；
- B4（Advanced）：少样本适配后模型。

不能把不同场景、物体或成功判据下的结果直接比较。

## 3. 正式场景

| 场景 | 变量 | 回合 |
|---|---|---:|
| S1 单物体抓放 | 3 个预定义初始位置 | ≥20 |
| S2 目标选择 | 2–3 个物体，语言指定其中一个 | ≥20 |
| S3 位置变化 | 训练布局内随机位置网格 | ≥20 |
| R1 光照 | 正常/偏暗/侧光 | 可选，各10 |
| R2 干扰物 | 无/少量杂物 | 可选，各10 |
| R3 指令改写 | 3 种等价表达 | 可选，各10 |

随机化种子和物体位置在运行前生成并保存。失败回合不得删除或重跑替代；技术故障标为 invalid，并保留原因。

## 4. 指标

- Task success rate 与 Wilson 95% CI；
- 正确目标选择率；
- grasp success、transport success、place success；
- task completion time；
- policy P50/P95 latency、峰值显存；
- safety rejection/abort count；
- 重试次数；
- 每千步相机过期、通信超时和设备断连次数。

成功定义：目标物体最终完全位于目标容器/区域内，并稳定 2 秒；不允许人工接触；在时间上限内完成。实际尺寸和时间上限在 pilot 后、正式试验前冻结。

## 5. 失败分类

- P：感知/目标选择错误；
- G：接近正确但抓取失败；
- A：动作语义、归一化或关节顺序错误；
- C：碰撞/安全层中止；
- L：延迟/通信/设备故障；
- T：长序列执行漂移；
- E：实验设置错误。

每个失败至少由视频和事件日志支持；无法判断时标 Unknown，不强行归因。

## 6. 数据与可视化

原始日志只追加；处理脚本从原始数据生成 `results/summary.csv`、成功率置信区间、延迟分布、阶段漏斗和失败类别图。图表必须标注 N、场景、checkpoint revision 和日期。

## 7. 风险登记表

| 风险/触发 | 影响 | 预防与调试 | 备选 |
|---|---|---|---|
| CUDA/Blackwell 不兼容 | 模型不能运行 | 官方 cu128；检查 sm_120、驱动 | 原生 Ubuntu/重装锁定 wheel/云 GPU |
| 16GB级显存不足 | OOM | BF16、单请求、关闭 CUDA graph | 远程 server；不做本地训练 |
| checkpoint 与 SO-101 契约不清 | 危险动作 | 校验 norm_stats、离线回放、单步 | 只做观测/动作分析，按官方例程适配 |
| WSL USB/串口不稳 | 真机中断 | 固定设备路径、断连测试 | 原生 Ubuntu 控制端，Windows/WSL 仅推理 |
| 采购超预算/延迟 | 真机延期 | Week 1 两渠道报价，早采购 | 缩减 SSD/云预算；继续离线工作 |
| 舵机零位/方向错误 | 机械损坏 | 单舵机、低速、无负载验证 | 更换件；限制工作区 |
| 相机顺序/颜色错误 | 策略失效 | schema/样例图/单测 | 单一适配入口，禁止散落转换 |
| 延迟过高 | 闭环失稳 | 测 P95、减小 action steps、异步采集 | 低速单步；LAN 推理 server |
| OOD 抓取失败 | 成功率低 | 训练分布内物体/视角、失败分析 | 收窄 MVP；Advanced 少样本适配 |
| 时间不足 | 文档/实验不完整 | 冻结范围、周审查 | 删除 ROS/微调，不删除安全与正式实验 |
| 数据不可复现 | 结果无价值 | revision/config/seed/原始日志 | 重新做预注册正式实验 |
| 安全事件 | 人/设备风险 | 急停、软物体、隔离区、监督 | 立即停止真机，回到 dry-run |

## 8. 保底路线

若策略无法可靠驱动 SO-101，仍交付：可复现 MolmoAct2 推理服务、真实双相机与关节状态采集、动作契约转换器、安全 dry-run、离线/仿真评估、真机遥操作 baseline、完整失败报告。只有真实完成的部分可写入简历；不把 dry-run 描述为自主控制。
