# V3 双机主动 SLAM 探索：代码与完整运行记录

本目录用于上传至 GitHub，可放在仓库的 `V3/` 下。压缩包内的顶层目录同样为 `V3/`。

## 内容

- `src/`：V3 主动 SLAM、B0–B4 消融、post44 修复版，以及所依赖的 V2 Q 表核心。
- `scripts/`、`worlds/fast/`、`models/`、`tests/`：运行与复核所需脚本、地图、机器人模型和测试。
- `experiments/active_slam_v3/`：原始 V3 试跑和报告。
- `experiments/active_slam_opt/formal/`：冻结的 44 组正式实验原始目录；报告在 `reports/phase44_2026-10-16/`。
- `experiments/active_slam_post44*/`：门槛与返航修复版的逐版试跑、诊断、启动异常及验证报告；当前 r11 报告在 `active_slam_post44_r11/reports/validation_2026-10-17/`。
- `SHA256SUMS.txt` 与 `PACKAGE_MANIFEST.json`：包内逐文件校验和与范围统计。

## 结果边界

44 组冻结正式实验：完成并返航 15，返航失败 8，超时 21。旧 ROI 为外接矩形。post44 使用可达空闲格加相邻首层墙格作为新 ROI，探索率严格大于 95%、新 ROI 已知率严格大于 90% 才返航。两种 ROI 百分比不能直接合并比较。

2026-10-17 的 r11 Gazebo 窗口演示：room/种子13/shared/B4，探索率 96.7044%、新 ROI 96.7036%，仿真探索 231.9 秒，最终 `return_failed`。原始结果与返航事件均在该次独立运行目录。走廊同版本种子7/independent/B3 为 timeout；修复版仍无充分证据证明总体超时或返航失败率下降。

`aborted_launcher/`、`aborted_runtime/` 和 `diagnostics/` 保留故障审计，**不计入**正式 44 组或有效试跑成功率。各运行目录的 `manifest.json` 保留地图、源码和配置校验信息；`source_snapshot/` 保留当次代码。

## 本地复现

在 ROS 2 Humble、Gazebo Classic、slam_toolbox、Nav2 已安装的虚拟机中，把本目录作为工作区根目录，运行：

```bash
source /opt/ros/humble/setup.bash
colcon build --packages-select dual_explore active_slam_explore --symlink-install
source install/setup.bash
DISPLAY=:0 python3 scripts/run_active_slam_post44.py --stage pilot --world worlds/fast/room.world --seed 13 --reward-mode shared --ablation B4 --gui true --max-wall-seconds 900
```

运行脚本会生成新的隔离目录，不会覆盖包内结果。Gazebo 图形窗口会在运行结束时自动关闭。
