# Document 3 — Resource & Environment Checklist

核查日期：2026-09-22。执行时以锁定 commit/revision 为准，不直接追随上游 main。

## 1. 软件基线

| 项目 | 规划版本 | 说明 |
|---|---|---|
| Host | Windows 11 + WSL2 Ubuntu 24.04；USB 不稳则原生 Ubuntu 24.04 | GPU 推理可先在 WSL2；实机串口需稳定 |
| Python | 3.11（允许 3.12） | 官方 MolmoAct2 pyproject 要求 >=3.11,<3.13 |
| 包管理 | uv，记录实际版本 | 依据官方 `uv sync` 路线 |
| PyTorch | 2.11.0 + cu128 | 官方当前 pin；不得擅自降级 |
| torchvision | 0.26.0 + cu128 | 与 PyTorch 配套 |
| Transformers | >=4.57,<4.58 | 官方约束 |
| LeRobot | 锁定支持 MolmoAct2 的 commit | 先用官方文档当前推荐分支 |
| ROS 2 | Jazzy（Enhanced） | 对应 Ubuntu 24.04 LTS |
| 仿真 | ManiSkill >=3.0.1 | 官方 sim_eval 依赖 |
| CUDA | wheel 自带 CUDA 12.8 runtime | 不要求单独安装 toolkit |
| Git/Git LFS | 当前稳定版并记录 | 代码与大文件管理 |
| Docker | 可选 | 实机 USB 阶段不作为唯一运行方式 |

驱动门槛按官方 README 当前说明：Linux >=570.26，Windows >=570.65；TASK-001 必须记录实际版本。RTX 50 系列检查 `torch.cuda.get_arch_list()` 含 `sm_120`。

## 2. 依赖与权重

- [allenai/molmoact2](https://github.com/allenai/molmoact2)：参考实现、推理服务、实验与 sim_eval。
- [MolmoAct2-SO100_101](https://huggingface.co/allenai/MolmoAct2-SO100_101)：主 checkpoint。
- [MolmoAct2](https://huggingface.co/allenai/MolmoAct2)：Advanced 适配起点。
- [LeRobot](https://github.com/huggingface/lerobot)：机器人、数据、训练和异步推理。
- [MolmoAct2 paper](https://arxiv.org/abs/2605.02881)：模型与实验依据。
- [ROS 2 Jazzy docs](https://docs.ros.org/en/jazzy/)：Enhanced 集成。
- [ManiSkill](https://github.com/haosulab/ManiSkill)：仿真协议验证。

权重预计约 22 GB/个（依据官方 README 的服务器说明）；保留至少 150 GB 空闲空间给 checkpoint、缓存、视频与环境。不要把权重、数据集或原始视频提交 Git。

## 3. 硬件与预算闸门

以下是规划估算，不是报价。采购前 TASK-008 至少比较 2 个供应渠道并确认运费、税、舵机型号和控制板。

| 项目 | 目标预算 |
|---|---:|
| SO-101 单臂套件/零件 | ¥3,200 |
| 第三视角 + 腕部相机 | ¥600 |
| 桌面固定、相机支架、工作区 | ¥450 |
| 电源、保险、实体急停/断电装置 | ¥350 |
| 工具、线材、备件 | ¥500 |
| SSD/存储预留 | ¥600 |
| Advanced 云 GPU 预留 | ¥800 |
| 价格波动/损坏应急 | ¥1,000 |
| **总计** | **¥7,500** |

采购硬门槛：含税到手预算不得超过 ¥8,000；若机械臂实际报价挤压安全件预算，先取消云 GPU/SSD升级，不削减急停、固定和备件。

MVP 不购买 leader arm。Advanced 数据采集可先用键盘/手柄/人工引导支持能力；若必须 leader arm，重新审批预算和范围。

## 4. 环境验收命令（TASK-004 时执行）

```bash
uv sync
uv run python -c "import torch, torchvision; print(torch.__version__, torchvision.__version__, torch.version.cuda, torch.cuda.is_available(), torch.cuda.get_device_name(0), torch.cuda.get_arch_list())"
```

预期不是写死字符串，而是验证：PyTorch/torchvision 与锁定文件一致、CUDA available、设备为 RTX 5080、架构包含 sm_120。

## 5. 采购与安装清单

- [ ] SO-101 BOM、舵机型号、控制板、电源规格核对
- [ ] 相机 UVC/Linux 兼容性、固定曝光能力核对
- [ ] 腕部相机重量和线缆拖拽评估
- [ ] 桌夹/底座、相机支架、软质测试物体
- [ ] 实体断电/急停、保险、隔离工作区
- [ ] USB 端口、供电和串口权限验证
- [ ] ≥150 GB 空闲存储
- [ ] 每项第三方许可证与模型使用条款记录

## 6. 可复现策略

提交 `uv.lock`/环境导出、上游 commit、checkpoint revision、配置 schema、设备清单和一键 smoke test。密钥只通过环境变量；`.env`、HF token、W&B key、权重、原始大视频必须被 `.gitignore` 排除。
