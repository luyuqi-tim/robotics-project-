# Document 4 — Learning Prerequisites

遵循 Just-in-Time Learning：每项学习都绑定任务和可验证练习。

## A. 已具备或快速复习

| 知识 | 目标 | 时间 | 小练习 | 对应任务 |
|---|---|---:|---|---|
| Python/模块化 | 能读懂 LeRobot 配置和写适配层 | 2h | 给 observation dataclass 写校验和单测 | TASK-006 |
| PyTorch 推理 | 理解 dtype/device/no_grad | 2h | 比较 fp32/bf16 张量显存 | TASK-004 |
| OpenCV | 正确处理 RGB/BGR、resize、视频 | 2h | 两相机采集并画时间戳 | TASK-018 |
| Git | 小步提交、分支和回滚 | 1h | 建 feature 分支并完成一次 PR | TASK-002 |

## B. 开始真机前必须学习

| 知识 | 必须掌握 | 资料 | 时间 | 验证练习 |
|---|---|---|---:|---|
| LeRobot 基础 | robot/config/dataset/policy、state/action schema | [LeRobot docs](https://huggingface.co/docs/lerobot/) | 6h | 读取样本并打印图像、state、action shape |
| SO-101 安全 | 零位、ID、方向、限位、电源和急停 | LeRobot SO-101 文档及供应商手册 | 6h | 断开负载完成单舵机低速测试 |
| MolmoAct2 契约 | checkpoint、norm_tag、camera order、action chunk | [官方仓库](https://github.com/allenai/molmoact2) | 5h | 对固定观测完成推理并验证 shape/range |
| 实验方法 | 预注册场景、trial、成功判据、置信区间 | 本项目测试文档 | 3h | 手工标注 10 个示例回合并计算一致性 |
| Linux 设备 | 串口、UVC、权限、日志 | Ubuntu/udev 文档 | 3h | 重插设备后稳定解析同一设备路径 |

## C. 边做边学

| 知识 | 何时学 | 时间 | 小练习 |
|---|---|---:|---|
| ROS 2 Jazzy node/topic/action/launch | MVP 闭环稳定后 | 10h | 图像→处理节点→日志节点 |
| 相机标定与 TF | 相机安装阶段 | 6h | 保存内参并测量静态外参复现误差 |
| VLA/flow matching | 分析模型与失败时 | 6h | 用图解释 action chunk 如何生成 |
| Profiling | 推理 baseline 阶段 | 4h | 报告 warmup、P50/P95、峰值显存 |
| LoRA/action expert tuning | Advanced | 10h | 20-step smoke training 并能恢复 checkpoint |

## D. 暂不学习

- 完整控制理论课程；
- SLAM、导航和移动机器人；
- 双臂协调；
- 强化学习全套算法；
- 从零训练 VLM/VLA；
- CUDA kernel 开发；
- 复杂抓取姿态规划和力控。

这些内容不会帮助当前 MVP 更快闭环。

## 学习完成原则

“看完教程”不算完成。只有小练习代码、终端输出或测试记录进入 `artifacts/learning/`，并能解释其与当前模块的关系，才标记完成。
