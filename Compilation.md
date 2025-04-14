# 格点软件编译与离线环境管理指南

## 1. 离线环境下的代码管理

### 1.1 离线下的 Git 仓库
使用中间仓库策略：
- 先在网络环境（如 IHEP）上 `git clone` 从 GitHub 获取代码
- 然后在离线环境（如 CSNS）上 `git clone` 从 IHEP 获取代码

### 1.2 离线下的 Python 虚拟环境
两种主要策略：
- 使用 venv 或 conda 来打包一个虚拟环境，带到离线环境去安装
- 安装自己的本地包：`pip install . --no-build-isolation`

### 1.3 离线下的 VSCode 与插件更新
关键配置：
- 在设置中启用 `"remote.downloadExtensionsLocally": true`，确保插件可在离线环境使用

---

## 2. 格点软件编译指南

### 2.1 Chroma 编译
- 参考 [USQCD Software](https://usqcd-software.github.io/) 的软件依赖层级
- 完整编译脚本可参考蒋师兄的安装方案：[Build-USQCD-SciDAC](https://github.com/SaltyChiang/Build-USQCD-SciDAC)

### 2.2 QUDA 编译

```bash
git pull # 更新 QUDA
mkdir build
cd build
export CUDA_HOME=/usr/local/cuda-12.3/ # 设定 CUDA 路径
cmake -DCMAKE_INSTALL_PREFIX="/home/shicj-wsl2/soft/quda-install-eead44e1" \
    -DQUDA_MPI=ON \
    -DQUDA_COVDEV=ON \
    -DQUDA_CLOVER_DYNAMIC=OFF \
    -DQUDA_CLOVER_RECONSTRUCT=OFF \
    -DQUDA_MULTIGRID=ON \ #需要开
    -DQUDA_DIRAC_DEFAULT_OFF=ON \
    -DQUDA_DIRAC_WILSON=ON \
    -DQUDA_DIRAC_CLOVER=ON \
    -DQUDA_DIRAC_STAGGERED=ON \
    -DQUDA_DIRAC_LAPLACE=ON \
    -DQUDA_GPU_ARCH=sm_75 \ # 架构
    ..
```

#### 编译常见问题及解决方案：

1. **环境配置问题**：编译前未正确配置环境，导致版本问题或库函数查找失败
   - 解决方法：
     - 检查已加载模块：`module li` / `module av`
     - 检查环境变量：
       ```
       echo $PATH
       echo $LIBRARY_PATH
       echo $LD_LIBRARY_PATH
       ```
     - **重要**：多层级编译时使用相同版本的底层依赖，特别是 OpenMPI/MPICH2、CUDA 等

2. **其他常见问题**：
   - CMAKE 的 `-D` 参数配置问题
   - 离线环境的网络依赖问题
   - CUDA 和 Git 版本依赖问题
   - OpenMPI 配置问题（将直接影响多卡性能）

### 2.3 PyQUDA 安装
- 确保安装与 QUDA 相同版本的 OpenMPI/MPICH2 和 CUDA，分别见他们安装的位置（包含 `bin/` 和 `lib/`的位置），重要！！！

- 按照 [PyQUDA](https://github.com/CLQCD/PyQUDA) 的说明安装：`export QUDA_PATH=XXX`
---

格点软件的编译和安装在离线环境中需要特别注意依赖项版本的一致性，尤其是跨多个软件层时。建议在开始前详细规划依赖关系，并在测试环境验证后再部署到生产环境。
