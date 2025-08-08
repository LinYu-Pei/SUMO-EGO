# Getting Started with SUMO-EGO / SUMO-EGO 入门指南

## 快速开始 / Quick Start

### 1. 环境准备 / Environment Setup

**安装SUMO / Install SUMO:**
```bash
# Ubuntu/Debian
sudo apt-get install sumo sumo-tools sumo-doc

# 或者从源码编译 / Or compile from source
# 详见: https://sumo.dlr.de/docs/Installing/index.html
```

**设置环境变量 / Set Environment Variables:**
```bash
export SUMO_HOME=/usr/share/sumo  # 根据你的安装路径调整 / Adjust to your installation path
```

### 2. 安装项目依赖 / Install Project Dependencies

```bash
git clone https://github.com/LinYu-Pei/SUMO-EGO.git
cd SUMO-EGO
pip install -r requirements.txt
python setup.py install
```

### 3. 运行第一个示例 / Run Your First Example

```bash
# 运行城市场景的协同仿真 / Run town scenario co-simulation
cd examples/town
python simulate_town_cosim.py
```

## 详细示例 / Detailed Examples

### 单EGO车辆仿真 / Single EGO Vehicle Simulation

```python
from libsumo_parallel import LibsumoParallelConnection
import random

def micro_callback(ego_id, init, edges):
    """
    微观仿真回调函数 / Microsimulation callback function
    
    Args:
        ego_id: EGO车辆ID / EGO vehicle ID
        init: 是否为初始化调用 / Whether this is initialization call
        edges: 可用边列表 / List of available edges
    """
    if init:
        # 初始化EGO车辆路径 / Initialize EGO vehicle route
        route_edges = ['edge1', 'edge2', 'edge3']  # 替换为实际边ID / Replace with actual edge IDs
        libsumo.route.add('ego_route', route_edges)
        libsumo.vehicle.add(ego_id, 'ego_route')
    
    # 获取当前仿真状态 / Get current simulation state
    vehicles = libsumo.vehicle.getIDList()
    
    # 在这里添加你的EGO车辆逻辑 / Add your EGO vehicle logic here
    if ego_id in vehicles:
        speed = libsumo.vehicle.getSpeed(ego_id)
        position = libsumo.vehicle.getPosition(ego_id)
        # 处理EGO车辆行为 / Handle EGO vehicle behavior

def meso_callback():
    """
    中观仿真回调函数 / Mesoscopic simulation callback function
    """
    # 处理中观仿真逻辑 / Handle mesoscopic simulation logic
    pass

# 创建仿真连接 / Create simulation connection
connection = LibsumoParallelConnection(micro_callback, meso_callback)

# 配置仿真参数 / Configure simulation parameters
connection.start_micro("path/to/micro.sumocfg")
connection.start_meso("path/to/meso.sumocfg")

# 运行仿真 / Run simulation
for step in range(1000):  # 运行1000步 / Run for 1000 steps
    connection.step()
```

### 多EGO车辆仿真 / Multiple EGO Vehicle Simulation

```python
# 启用多EGO模式 / Enable multi-EGO mode
connection.multi_ego = True

def multi_ego_callback(ego_ids, init, edges):
    """
    多EGO车辆回调函数 / Multi-EGO vehicle callback function
    
    Args:
        ego_ids: EGO车辆ID列表 / List of EGO vehicle IDs
        init: 是否为初始化调用 / Whether this is initialization call
        edges: 可用边列表 / List of available edges
    """
    for i, ego_id in enumerate(ego_ids):
        if init:
            # 为每个EGO车辆设置不同路径 / Set different routes for each EGO vehicle
            route_name = f'ego_route_{i}'
            route_edges = get_route_for_ego(i)  # 自定义函数 / Custom function
            libsumo.route.add(route_name, route_edges)
            libsumo.vehicle.add(ego_id, route_name)
        
        # 处理每个EGO车辆的行为 / Handle behavior for each EGO vehicle
        if ego_id in libsumo.vehicle.getIDList():
            # 个性化控制逻辑 / Personalized control logic
            pass
```

## 配置文件示例 / Configuration File Examples

### SUMO配置文件 / SUMO Configuration File

```xml
<!-- micro.sumocfg -->
<configuration>
    <input>
        <net-file value="network.net.xml"/>
        <route-files value="routes.rou.xml"/>
    </input>
    <time>
        <begin value="0"/>
        <end value="3600"/>
        <step-length value="0.1"/>
    </time>
    <processing>
        <collision.action value="warn"/>
    </processing>
</configuration>
```

## 性能优化建议 / Performance Optimization Tips

**中文建议:**
1. **合理设置仿真步长**: 微观仿真使用0.1s，中观仿真可使用1s
2. **优化EGO区域大小**: 根据应用需求调整微观仿真区域
3. **批量处理**: 使用`aggregate_runs.py`进行批量仿真
4. **内存管理**: 定期清理不需要的车辆数据

**English Tips:**
1. **Appropriate Step Size**: Use 0.1s for microsimulation, 1s for mesoscopic
2. **Optimize EGO Region**: Adjust microsimulation area based on application needs
3. **Batch Processing**: Use `aggregate_runs.py` for batch simulations
4. **Memory Management**: Regularly clean up unnecessary vehicle data

## 故障排除 / Troubleshooting

### 常见问题 / Common Issues

**Q: SUMO_HOME环境变量未设置 / SUMO_HOME environment variable not set**
```bash
export SUMO_HOME=/path/to/sumo  # Linux/Mac
set SUMO_HOME=C:\path\to\sumo   # Windows
```

**Q: TraCI连接错误 / TraCI connection error**
- 确保SUMO正确安装 / Ensure SUMO is properly installed
- 检查端口是否被占用 / Check if ports are available
- 验证配置文件路径 / Verify configuration file paths

**Q: 性能问题 / Performance issues**
- 减少微观仿真区域 / Reduce microsimulation area
- 增加仿真步长 / Increase step length
- 使用更少的EGO车辆 / Use fewer EGO vehicles

## 进阶主题 / Advanced Topics

### 自定义车辆行为 / Custom Vehicle Behavior
- 实现自定义驾驶模型 / Implement custom driving models
- 集成外部控制算法 / Integrate external control algorithms
- 添加传感器模拟 / Add sensor simulation

### 与其他工具集成 / Integration with Other Tools
- 连接到ROS (Robot Operating System)
- 集成机器学习框架 / Integrate ML frameworks
- 连接到可视化工具 / Connect to visualization tools

## 更多资源 / Additional Resources

- [SUMO官方文档 / SUMO Official Documentation](https://sumo.dlr.de/docs/)
- [TraCI接口文档 / TraCI Interface Documentation](https://sumo.dlr.de/docs/TraCI.html)
- [项目学术论文 / Academic Paper](https://doi.org/10.1109/ACCESS.2023.3284316)