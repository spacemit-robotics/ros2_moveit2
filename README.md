# moveit2_run

## 项目简介

`moveit2_run` 是面向 ROS 2 Humble 的 MoveIt 2 运行与仿真示例组件，提供 Universal Robots（UR）机械臂相关的 Gazebo 仿真、K3 Com260 RISC-V 64 端 MoveIt 规划运行，以及 PC 端 RViz 可视化启动流程。

本组件主要解决在 PC 主机进行仿真与可视化、在远程 K3 Com260 板端运行 MoveIt 规划能力时的启动配置与资源组织问题，便于快速验证机械臂模型、控制链路和运动规划流程。

## 功能特性

支持：

- ROS 2 Humble 环境下的 MoveIt 2 启动配置。
- PC 主机启动 Gazebo 仿真环境。
- 远程 K3 Com260 RISC-V 64 机器启动 MoveIt 规划服务。
- PC 主机启动 RViz 进行机械臂状态与规划结果可视化。
- UR 机械臂相关的 `urdf`、`srdf`、`config`、`launch`、`rviz`、`meshes` 等资源组织。

暂不支持或需用户自行适配：

- 非 ROS 2 Humble 版本的完整兼容性验证。
- 非 Ubuntu 22.04 PC 环境的完整兼容性验证。
- 非 K3 Com260 RISC-V 64 目标平台的板端运行验证。
- 真实机械臂硬件接入流程的完整说明。

## 快速开始

推荐使用 PC 主机运行 Gazebo 仿真和 RViz，可使用远程 K3 Com260 RISC-V 64 机器运行 MoveIt 规划相关节点。最短启动路径如下：

1. PC 主机启动 Gazebo 仿真。
2. 远程 K3 Com260 RISC-V 64 机器启动 MoveIt。
3. PC 主机启动 RViz 查看和操作规划流程。

### 环境准备

PC 主机推荐环境：

- Ubuntu 22.04
- ROS 2 Humble
- Gazebo / RViz / MoveIt 2 相关依赖

PC 主机依赖安装示例：

```bash
sudo apt install ros-humble-realtime-tools ros-humble-hardware-interface \
ros-humble-ros2-control-test-assets ros-humble-filters ros-humble-ompl \
ros-humble-moveit-visual-tools ros-humble-moveit-servo \
ros-humble-ros-gz-sim ros-humble-ros-gz-bridge ros-humble-ros-gz-interfaces \
ros-humble-gz-ros2-control ros-humble-ign-ros2-control

sudo apt install librange-v3-dev
```

远程 K3 Com260 RISC-V 64 机器需要安装 MoveIt 2 运行相关依赖，例如：

```bash
sudo apt install ros-humble-moveit-core \
ros-humble-moveit-ros-control-interface \
ros-humble-moveit-ros-planning-interface
```

根据实际镜像和功能裁剪情况，可能还需要补充安装 `moveit_ros_move_group`、`moveit_servo`、`moveit_kinematics`、`moveit_planners_ompl`、`warehouse_ros_sqlite`、`ur_controllers` 等运行依赖。

### 构建编译

在 ROS 2 工作空间中放置本组件后，使用 `colcon` 构建：

```bash
colcon build --packages-select moveit2_run
source install/setup.bash
```

如果本组件作为 SDK 的一部分集成构建，请优先使用 SDK 提供的构建脚本和目标配置。

### 运行示例

PC 主机启动 Gazebo 仿真：

```bash
ros2 launch moveit2_run ur_sim_gazebo_launch.py
```

远程 K3 Com260 RISC-V 64 机器启动 MoveIt：

```bash
ros2 launch moveit2_run ur_moveit_launch.py
```

PC 主机启动 RViz：

```bash
ros2 launch moveit2_run ur_moveit_rviz_launch.py
```

## 详细使用

详细使用说明后续参考官方文档，包括但不限于：

- ROS 2 官方文档
- MoveIt 2 官方文档
- Gazebo / ros_gz 官方文档
- Universal Robots ROS 2 驱动与控制相关文档
- Spacemit K3 Com260 SDK 相关文档

## 常见问题

- **PC 主机推荐使用什么系统？**
  推荐 Ubuntu 22.04，并使用 ROS 2 Humble。

- **为什么 PC 和 K3 Com260 需要分别启动不同 launch 文件？**
  PC 侧主要负责 Gazebo 仿真和 RViz 可视化，K3 Com260 侧负责运行目标平台上的 MoveIt 规划相关节点，便于模拟实际板端部署场景。

- **启动时提示缺少 ROS 2 包怎么办？**
  按照“环境准备”章节补齐 PC 或板端依赖；如果仍然缺包，请结合 `package.xml` 中声明的依赖和实际启动日志继续安装对应 `ros-humble-*` 包。

- **RViz 中看不到机器人或规划结果怎么办？**
  确认 Gazebo 仿真、K3 Com260 侧 MoveIt 节点和 PC 侧 RViz 均已启动，并检查多机 ROS 2 网络配置、`ROS_DOMAIN_ID`、DDS 通信和时间同步设置。

- **板端依赖包名称和 PC 侧完全一致吗？**
  不一定。板端镜像可能经过裁剪，部分依赖需要根据仓库源和镜像版本确认可用包名。

## 版本与发布

当前版本信息以 `package.xml` 中的 `<version>` 字段为准。

## 贡献方式

欢迎通过提交 issue、合并请求或补充文档的方式参与贡献。提交前建议：

- 遵循仓库代码规范和 ROS 2 包结构约定。
- 确认新增 launch、配置和模型资源可在目标环境中正常运行。
- 补充必要的使用说明、测试说明和依赖说明。
- 保持许可证声明和第三方资源来源清晰。

贡献者与维护者名单见：`CONTRIBUTORS.md`

## License

本组件源码文件头声明为 Apache-2.0，最终以本目录 `LICENSE` 文件为准。
