# EGO-Centric SUMO / EGO中心的SUMO交通仿真

## 项目简介 / Project Overview

**中文说明**: 这个项目是一个基于SUMO (城市交通仿真软件) 的EGO车辆中心化交通仿真系统。该项目通过混合仿真方法，在大规模城市交通场景中提升围绕一个或多个EGO车辆的仿真速度。

**English**: This project is an EGO-centric traffic simulation system based on SUMO (Simulation of Urban Mobility). It aims to accelerate traffic simulation around one or multiple EGO vehicles in large urban traffic scenarios using a hybrid simulation approach.

## 核心技术 / Core Technology

### 混合仿真方法 / Hybrid Simulation Approach

**中文**:
- **微观仿真 (Microsimulation)**: 在EGO车辆周围使用全尺度微观仿真，提供详细的车辆行为建模
- **中观仿真 (Mesoscopic)**: 网络的其余部分使用SUMO的中观模型进行仿真，提高计算效率
- **动态切换**: 系统能够根据EGO车辆位置动态调整仿真精度

**English**:
- **Microsimulation**: Full-scale microsimulation around EGO vehicle(s) for detailed vehicle behavior modeling
- **Mesoscopic**: Rest of the network simulated with SUMO's mesoscopic model for computational efficiency  
- **Dynamic Switching**: System dynamically adjusts simulation fidelity based on EGO vehicle position

![Hybrid Simulation Approach](https://ieeexplore.ieee.org/ielx7/6287639/10005208/10146270/graphical_abstract/access-gagraphic-3284316.jpg)

## 项目优势 / Project Benefits

**中文**:
1. **性能优化**: 相比纯微观仿真，显著提升大规模场景的仿真速度
2. **精度保持**: 在关键区域(EGO车辆周围)保持高精度仿真
3. **可扩展性**: 支持多个EGO车辆的同时仿真
4. **灵活性**: 可适应不同规模的城市交通网络

**English**:
1. **Performance**: Significantly faster than pure microscopic simulation for large scenarios
2. **Accuracy**: Maintains high-fidelity simulation around critical areas (EGO vehicles)
3. **Scalability**: Supports multiple EGO vehicles simultaneously
4. **Flexibility**: Adaptable to different scales of urban traffic networks

## 安装与设置 / Installation & Setup

### 依赖要求 / Requirements
```bash
pip install -r requirements.txt
```

Dependencies include:
- numpy~=1.23.4
- matplotlib~=3.5.3
- traci~=1.15.0
- libsumo~=1.15.0
- sumolib~=1.15.0

### 环境变量 / Environment Variables
Set the SUMO_HOME environment variable:
```bash
export SUMO_HOME=/path/to/sumo
```

### 安装 / Installation
```bash
python setup.py install
```

## 使用示例 / Usage Examples

### 基本用法 / Basic Usage
```python
from libsumo_parallel import LibsumoParallelConnection

# Define callbacks for micro and mesoscopic simulations
def micro_callback(ego_id, init, edges):
    # Handle microsimulation logic around EGO vehicle
    pass

def meso_callback():
    # Handle mesoscopic simulation logic
    pass

# Create connection
connection = LibsumoParallelConnection(micro_callback, meso_callback)
```

### 运行示例 / Run Examples
```bash
# Run town scenario with co-simulation
python examples/town/simulate_town_cosim.py

# Run with multiple EGO vehicles
python examples/town/simulate_town_cosim_multi.py

# Compare with pure microscopic simulation
python examples/town/simulate_town_micro.py
```

## 文件结构 / File Structure

```
SUMO-EGO/
├── libsumo_parallel.py      # 核心并行仿真库 / Core parallel simulation library
├── aggregate_runs.py        # 批量运行脚本 / Batch execution script
├── evaluate_results.py      # 结果评估工具 / Results evaluation tools
├── examples/
│   └── town/               # 城市场景示例 / Town scenario examples
├── requirements.txt        # 依赖列表 / Dependencies
└── setup.py               # 安装脚本 / Installation script
```

## 应用场景 / Use Cases

**中文**:
- **自动驾驶车辆测试**: 在大规模交通环境中测试自动驾驶算法
- **交通影响分析**: 分析特定车辆对整体交通流的影响
- **智能交通系统**: 开发和测试车联网(V2X)应用
- **交通安全研究**: 研究特定车辆行为的安全性影响

**English**:
- **Autonomous Vehicle Testing**: Test autonomous driving algorithms in large-scale traffic environments
- **Traffic Impact Analysis**: Analyze the impact of specific vehicles on overall traffic flow
- **Intelligent Transportation Systems**: Develop and test V2X applications
- **Traffic Safety Research**: Study safety implications of specific vehicle behaviors

## 学术背景 / Academic Background

This project is based on the research paper:

**B. Varga, T. Ormándi and T. Tettamanti**, "EGO-Centric, Multi-Scale Co-Simulation to Tackle Large Urban Traffic Scenarios," in *IEEE Access*, vol. 11, pp. 57437-57447, 2023, doi: [10.1109/ACCESS.2023.3284316](https://doi.org/10.1109/ACCESS.2023.3284316)

## 许可证 / License

MIT License - 详见 LICENSE 文件 / See LICENSE file for details

## 贡献 / Contributing

欢迎提交问题和拉取请求！/ Issues and pull requests are welcome!

## 联系方式 / Contact

- Author: Balazs Varga
- Email: varga.balazs@kjk.bme.hu
