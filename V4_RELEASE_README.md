# V4 优化完成冻结版（单机主动 SLAM 探索）

发布标识：**V4_optimized_frozen_2026-10-18**。

[下载完整冻结包](V4_optimized_frozen_2026-10-18.zip) · [SHA256 校验文件](V4_optimized_frozen_2026-10-18.zip.sha256)

本包包含运行源码、固定参数/世界模板、本地 tf2/Cyclone 依赖、复现指南、完整实验报告、覆盖率曲线、建图文件与原始日志；旧版本的调试数据和源码快照独立归档。当前发布范围是已验证的单机版本。

## 功能与修复

激光与里程计 → slam_toolbox 与动态 TF → frontier 聚类 → 安全观测位姿 → 可见未知格代理收益与 Nav2 路径代价 → Nav2 全局规划与 RPP 路径跟随 → 地图更新。

修复 TF 锁顺序死锁、持续旋转的控制器配置、返航坐标/完成核验，以及 SDF 保存状态覆盖随机障碍坐标的问题。探索率严格 >95% 且 ROI 已知率 >90% 后返航；距实际部署点 ≤0.35 米才计成功。任务预算为 700 秒仿真时间。

## 发布时结果

| 类型 | 地图/策略种子 | 探索率 | ROI | 总任务仿真时间 | 返航误差 |
|---|---|---:|---:|---:|---:|
| 预先固定验收 | 7/11 | 95.73% | 96.46% | 583 秒 | 0.198 米 |
| 预先固定验收 | 9/13 | 95.22% | 95.39% | 618 秒 | 0.195 米 |
| 预先固定验收 | 101/501 | 98.26% | 98.54% | 689 秒 | 0.204 米 |
| 预先固定验收 | 202/602 | 95.59% | 95.73% | 536 秒 | 0.141 米 |
| 独立随机补充 | 375954/868225 | 95.22% | 95.34% | 587 秒 | 0.170 米 |

原四组记录 132 次导航结果，全部成功，目标超时为 0。补充测试独立记录，不更改原四组验收。五次功能测试均成功，不代表任意地图都能成功。

## 复现

Ubuntu 22.04 x86_64、Python 3.10、ROS 2 Humble、Gazebo Classic 11。准确依赖版本与安装说明在压缩包内 `REPRODUCIBILITY.md` 和 `environment/`。

```bash
sha256sum -c V4_optimized_frozen_2026-10-18.zip.sha256
unzip V4_optimized_frozen_2026-10-18.zip
cd V4_optimized_frozen_2026-10-18
python3 scripts/verify_release.py
python3 scripts/check_environment.py
bash scripts/build_isolated.sh
bash scripts/run_gui.sh --map-seed 7 --policy-seed 11
```

其他种子使用同一命令，显式记录地图种子与策略种子。原四组重新验收及汇总：

```bash
python3 scripts/run_validation_suite.py
python3 scripts/export_reproduction_report.py
```

**种子影响布局、目标顺序与耗时。固定种子和版本可复现输入，但 ROS/Gazebo 的异步调度不能保证轨迹、耗时和结果逐次一致。** 当前是启发式主动 SLAM 探索导航系统，本版没有新增强化学习训练或更新 Q 表。

本包在换目录后完成四包干净构建、29 项 Python 测试、tf2 回归和地图生成哈希校验。详细原始结果、统一配色 8 倍 PNG、PGM/YAML 和覆盖率曲线均在包内。发布只冻结当前单机成果，双机协同与其他地图族需另做实验验证。
