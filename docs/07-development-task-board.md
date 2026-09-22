# Document 7 — Development Task Board

状态：`TODO` / `IN PROGRESS` / `BLOCKED` / `DONE`。只有提供验收证据后才能改为 DONE。

## M0 环境与决策

| ID | 任务 | 依赖 | 预计 | 交付/验收 |
|---|---|---|---:|---|
| TASK-001 | 采集宿主机、GPU、驱动、WSL/Ubuntu、磁盘基线 | — | 1h | `artifacts/env/system-baseline.md`；命令输出完整 |
| TASK-002 | 建立分支、commit、PR 与标签规范 | 001 | 1h | CONTRIBUTING；完成示例 PR |
| TASK-003 | 锁定上游 MolmoAct2/LeRobot commit 与 checkpoint revision | 001 | 2h | `configs/versions.yaml` |
| TASK-004 | 创建 uv/Python/cu128 环境并验证 sm_120 | 003 | 3h | smoke 命令全通过 |
| TASK-005 | 建立 pytest、lint 和最小 CI | 004 | 2h | 本地/CI green |
| TASK-006 | 定义 config、Observation、ActionChunk schema | 003 | 2h | schema tests 通过 |

## M1 模型 Baseline 与采购

| ID | 任务 | 依赖 | 预计 | 交付/验收 |
|---|---|---|---:|---|
| TASK-007 | 下载并校验 SO100_101 checkpoint | 004 | 2h | revision、文件清单、哈希/缓存位置 |
| TASK-008 | 完成 SO-101 BOM 与两渠道报价 | — | 3h | 含税到手总预算 ≤¥8,000 |
| TASK-009 | 下单机械臂、安全件、相机和支架 | 008 | 1h | 订单/BOM 脱敏记录 |
| TASK-010 | 构造固定双图像和 state 测试 fixture | 006 | 2h | fixture 可重复加载 |
| TASK-011 | 跑通单次 BF16 离线推理 | 007,010 | 3h | 输出 shape/dtype/range 有记录 |
| TASK-012 | 验证 norm_tag、camera order、关节顺序 | 011 | 3h | 契约说明和断言 |
| TASK-013 | 运行 20 次稳定性与延迟/显存 benchmark | 012 | 2h | JSON + P50/P95 + peak VRAM |
| TASK-014 | 建立 policy server/client smoke test | 013 | 2h | 20 次请求无崩溃 |

## M2 硬件 Bring-up

| ID | 任务 | 依赖 | 预计 | 交付/验收 |
|---|---|---|---:|---|
| TASK-015 | 按 BOM 装配 SO-101 并检查线缆/紧固 | 009 | 5h | 照片与机械 checklist |
| TASK-016 | 配置舵机 ID、零位、方向和软限位 | 015 | 5h | 逐关节低速测试 |
| TASK-017 | 实现并验证实体急停、断连停机 | 016 | 3h | 三类故障测试通过 |

## M3 观测管线

| ID | 任务 | 依赖 | 预计 | 交付/验收 |
|---|---|---|---:|---|
| TASK-018 | 枚举并稳定映射两路 UVC 相机 | 009 | 2h | 重插/重启映射稳定 |
| TASK-019 | 固定曝光、分辨率、帧率和色彩转换 | 018 | 2h | 保存原图与 RGB 样例 |
| TASK-020 | 读取 SO-101 joint/gripper state | 016 | 2h | 10 分钟无异常值 |
| TASK-021 | 实现 Observation Assembler 和 freshness check | 012,019,020 | 4h | unit tests |
| TASK-022 | 实现 30 分钟录制与回放 | 021 | 3h | 回放与原始时间戳一致 |

## M4 安全闭环

| ID | 任务 | 依赖 | 预计 | 交付/验收 |
|---|---|---|---:|---|
| TASK-023 | 实现 Action Adapter 与反归一化审计 | 012 | 3h | golden tests |
| TASK-024 | 实现关节范围/步长/速率/NaN 检查 | 017,023 | 4h | 边界单测 |
| TASK-025 | 实现 IDLE/ARMED/RUNNING/ABORT 状态机 | 024 | 3h | 状态迁移单测 |
| TASK-026 | 完成不通电 dry-run 和动作可视化 | 021,025 | 2h | 人工审计报告 |
| TASK-027 | 完成架空、低速、单步执行 | 026 | 3h | 无物体安全录像 |
| TASK-028 | 接入有限 action chunk 闭环 | 027,014 | 3h | 重新观测正常 |
| TASK-029 | 注入相机/策略/串口超时故障 | 028 | 2h | 全部进入 ABORT |
| TASK-030 | 冻结真机安全 checklist | 029 | 1h | reviewer 签字/确认 |

## M5 MVP 实验

| ID | 任务 | 依赖 | 预计 | 交付/验收 |
|---|---|---|---:|---|
| TASK-031 | 实现 episode logger 与不可变 metadata | 022,028 | 3h | 每回合文件完整 |
| TASK-032 | 定义成功判据、时间上限和 invalid 规则 | 030 | 2h | 正式实验前冻结 |
| TASK-033 | 设计物体、容器和位置网格 | 032 | 2h | 场景表和照片 |
| TASK-034 | 运行小规模 pilot，修正非评价性 bug | 031,033 | 3h | pilot 报告 |
| TASK-035 | 冻结正式实验配置与随机序列 | 034 | 1h | config hash |
| TASK-036 | 完成 S1 ≥20 回合 | 035 | 3h | 原始日志/视频 |
| TASK-037 | 完成 S2 ≥20 回合 | 036 | 3h | 原始日志/视频 |
| TASK-038 | 完成 S3 ≥20 回合 | 037 | 3h | 原始日志/视频 |
| TASK-039 | 计算指标、CI、阶段漏斗和失败分类 | 036–038 | 4h | 可重建图表/表格 |

## M6 Enhanced ROS 2（可推迟）

| ID | 任务 | 依赖 | 预计 | 交付/验收 |
|---|---|---|---:|---|
| TASK-040 | 建 ROS 2 Jazzy workspace/package | 039 | 2h | colcon test 通过 |
| TASK-041 | 封装 camera/state/driver nodes | 040 | 3h | topic schema 验证 |
| TASK-042 | 封装 policy client 与 safety supervisor | 041 | 3h | 与 direct pipeline golden test 一致 |
| TASK-043 | 实现 RunTask action、diagnostics 和 launch | 042 | 3h | 一条 launch 启动 |
| TASK-044 | rosbag2 录制并复现实验 | 043 | 2h | 回放指标一致 |

## M7 交付与 Advanced Gate

| ID | 任务 | 依赖 | 预计 | 交付/验收 |
|---|---|---|---:|---|
| TASK-045 | 在干净环境执行复现测试 | 039 | 3h | 全新环境记录 |
| TASK-046 | 完成 README 安装、运行、测试与安全说明 | 045 | 3h | 无口头步骤 |
| TASK-047 | 生成架构图、结果图和失败案例 | 039 | 2h | 数据可追溯 |
| TASK-048 | 录制未剪切核心 Demo 与讲解版视频 | 045 | 3h | 指令、动作、结果同框 |
| TASK-049 | 完成技术总结和限制 | 047 | 2h | 不隐藏失败 |
| TASK-050 | 审计许可证、模型/数据 attribution | 046 | 2h | NOTICE/引用齐全 |
| TASK-051 | 基于真实结果写简历 bullet 草案 | 048,049 | 1h | 每个数字可追溯 |
| TASK-052 | 评审是否进入 100–300 条数据微调 | 039,050 | 2h | go/no-go ADR |

## Git 规范

- 分支：`feat/TASK-xxx-short-name`、`fix/TASK-xxx-short-name`、`docs/TASK-xxx-short-name`。
- Commit：`feat(policy): TASK-023 add action adapter`。
- 一个 PR 聚焦一个任务或一个无法拆开的紧密任务组。
- PR 必含：目的、变更、运行命令、测试结果、风险、文档更新。
- `main` 始终可运行；实验性 checkpoint/数据不进入 Git history。

## 计划中的仓库结构

```text
configs/            版本、机器人、相机、任务和安全配置
src/io/             相机与录制
src/robot/          SO-101 adapter/executor
src/policy/         observation、client、action adapter
src/safety/         checks 和状态机
src/evaluation/     logger、metrics、failure taxonomy
ros2_ws/src/        Enhanced ROS 2 packages
scripts/            安装、诊断、运行、评估
tests/              unit/integration/smoke
artifacts/          小型可追溯报告；大文件外置
docs/               设计、路线、实验报告
```
