# Baseline v1：双机器人共享 Q-learning 探索

冻结日期：2026-10-13。此目录保存已在 ROS 2 Humble 虚拟机运行的实验基线。

- 压缩包：[`dual_explore_baseline_v1_2026-10-13.zip`](dual_explore_baseline_v1_2026-10-13.zip)
- SHA-256：`d6c1407f3eefc644dae7d397db2241cc5d00cccf3200f6995ac68bb58065d050`
- 包内 `BASELINE.md` 包含环境、启动方法、算法参数及限制；`MANIFEST.json` 包含逐文件校验值。
- 内容：带中文注释的 ROS 2 源码、Gazebo 场景、测试、代码阅读指南，以及冻结时 Q 表和 CSV 的运行快照。
- 验证：核心测试 8/8 通过；归档和逐文件校验通过。

这是共享 Q-learning 协同探索原型，不是 SLAM 主动定位或占据地图构建。运行快照不是干净训练的初始状态。
