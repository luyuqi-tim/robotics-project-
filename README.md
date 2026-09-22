# MolmoAct2-SO101

语言驱动桌面操作机器人的仿真、真机部署与鲁棒性评估。

> 状态：Phase A 规划完成，尚未开始 TASK-001。所有性能数字必须来自后续真实测试。

## 项目目标

在单臂 SO-101、第三视角相机和腕部相机上部署 MolmoAct2，使机器人根据自然语言完成桌面抓取、放置与目标选择；建立可复现环境、安全执行层、实验记录和失败分析体系。

- 预算上限：¥8,000 CNY
- 时间投入：每天 08:00–10:00，约 14 小时/周
- 计划周期：12 周（10 周执行 + 2 周缓冲）
- 主机：Windows + Ubuntu/WSL2、NVIDIA RTX 5080、32 GB RAM
- 策略：仿真/离线验证优先，单臂真机为 MVP，少样本适应为 Advanced

## 文档

1. [Project Proposal](docs/01-project-proposal.md)
2. [Technical Design](docs/02-technical-design.md)
3. [Resource & Environment Checklist](docs/03-resource-environment-checklist.md)
4. [Learning Prerequisites](docs/04-learning-prerequisites.md)
5. [Implementation Roadmap](docs/05-implementation-roadmap.md)
6. [Testing & Risk Plan](docs/06-testing-risk-plan.md)
7. [Development Task Board](docs/07-development-task-board.md)

## MVP 完成标准

- 可复现地安装并锁定 MolmoAct2 推理环境。
- 在 RTX 5080 上完成 BF16 推理烟雾测试并记录显存、延迟和失败日志。
- SO-101 完成装配、标定、关节限位和急停验证。
- 两路相机、机器人状态、语言指令能组成符合模型约定的观测。
- 闭环完成至少 3 类任务：单物体抓放、目标选择、位置变化抓放。
- 每类任务至少 20 次正式试验，报告成功率、完成时间、推理延迟和失败类型。
- 从全新环境按 README 能复现核心 Demo；发布 Demo 视频与技术总结。

## 当前第一项任务

**TASK-001：记录宿主机、GPU、驱动、WSL/Ubuntu 和存储基线。**

完成并提供可验证输出前，不把任务标记为 Done。参见 [Task Board](docs/07-development-task-board.md)。

## 上游资料

- [Ai2 MolmoAct2](https://github.com/allenai/molmoact2)
- [MolmoAct2 paper](https://arxiv.org/abs/2605.02881)
- [MolmoAct2-SO100_101 checkpoint](https://huggingface.co/allenai/MolmoAct2-SO100_101)
- [LeRobot](https://github.com/huggingface/lerobot)
- [ROS 2 Jazzy](https://docs.ros.org/en/jazzy/)
- [ManiSkill](https://github.com/haosulab/ManiSkill)

## 事实核查

技术链接与上游版本于 **2026-09-22** 核查。上游会变化；每个里程碑开始前重新检查 release、模型卡和硬件说明。
