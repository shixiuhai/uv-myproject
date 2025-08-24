# UV 工具使用指南

## 简介
uv 是一个高效的 Python 包管理工具，提供快速依赖解析和安装功能。

## 安装
```bash
pip install uv
```

## 完整命令集

### 项目开发命令

#### `uv run` - 运行命令或脚本
```bash
# 运行项目中的脚本
uv run dev
# 带参数运行
uv run test --verbose
```

#### `uv init` - 创建新项目
```bash
# 创建新Python项目
uv init myproject
# 指定Python版本
uv init myproject --python=3.10
```

#### `uv version` - 管理项目版本
```bash
# 查看当前版本
uv version
# 设置新版本
uv version 1.2.0
```

#### `uv tree` - 显示依赖树
```bash
# 显示完整依赖树
uv tree
# 限制显示深度
uv tree --depth=2
```

#### `uv format` - 代码格式化
```bash
# 格式化整个项目
uv format .
# 检查但不修改
uv format --check src/
```

### 依赖管理命令

#### `uv add` - 添加依赖
```bash
# 添加常规依赖
uv add requests
# 添加开发依赖
uv add pytest --dev
# 指定版本
uv add numpy==1.26.0
# 从Git仓库安装并设置为可编辑模式
uv add git+https://github.com/用户名/abc.git#egg=abc --editable=./abc
1️⃣ uv add

universal-venv（uv）用来添加依赖包的命令，相当于 pip install。

会把依赖写入 pyproject.toml 的 [tool.uv.dependencies] 或 [tool.uv.dev-dependencies]。

2️⃣ git+https://github.com/用户名/abc.git

从 Git 仓库安装依赖，而不是 PyPI。

格式：git+<git仓库地址>。

支持 HTTPS 或 SSH。

3️⃣ #egg=abc

包名声明，告诉 uv 生成的包名是 abc。

必须有，否则 uv 不知道包名。

4️⃣ --editable=./abc

等同于 pip 的 -e 参数。

将包安装为 可编辑模式（editable mode）。

安装后对 ./abc 的修改会即时生效，无需重新安装。
```

#### `uv remove` - 移除依赖
```bash
# 移除包
uv remove requests
# 移除开发依赖
uv remove pytest --dev
```

#### `uv sync` - 同步环境
```bash
# 同步所有依赖
uv sync
# 仅同步生产依赖
uv sync --production
```

#### `uv lock` - 生成lockfile
```bash
# 更新lockfile
uv lock
# 强制重新生成
uv lock --force
```

#### `uv export` - 导出依赖
```bash
# 导出为requirements.txt
uv export > requirements.txt
# 导出生产环境依赖
uv export --production > prod-requirements.txt
```

### 包管理命令

#### `uv tool` - 运行包命令
```bash
# 运行black格式化
uv tool black --check .
# 运行flake8检查
uv tool flake8 src/
```

#### `uv pip` - pip兼容接口
```bash
# 安装包(兼容pip语法)
uv pip install package
# 显示已安装包
uv pip list
```

#### `uv build` - 构建包
```bash
# 构建源代码分发
uv build --sdist
# 构建wheel
uv build --wheel
```

#### `uv publish` - 发布包
```bash
# 发布到PyPI
uv publish
# 发布到测试服务器
uv publish --repository testpypi
```

### Python环境命令

#### `uv python` - 管理Python
```bash
# 列出可用Python版本
uv python list
# 安装指定版本
uv python install 3.11
```

#### `uv venv` - 创建虚拟环境
```bash
# 创建默认虚拟环境
uv venv .venv
# 指定Python版本
uv venv .venv --python=3.10
```

### 工具命令

#### `uv cache` - 缓存管理
```bash
# 列出所有缓存内容
uv cache list
# 清理过期缓存
uv cache clean --expired
# 显示缓存状态
uv cache info
```

#### `uv self` - 管理程序
```bash
# 更新uv自身
uv self update
# 显示版本
uv self version
```

### Git仓库安装详解

通过`uv add`可以从Git仓库安装Python包并设置为可编辑模式:

```bash
uv add git+https://github.com/用户名/仓库.git#egg=包名 --editable=./本地目录
```

参数说明:
- `git+https://...`: Git仓库URL，必须包含`git+`前缀
- `#egg=包名`: 指定Python包名(必须)
- `--editable=./本地目录`: 设置为可编辑模式并指定本地克隆目录

工作流程:
1. 命令会克隆仓库到`./本地目录`
2. 以开发模式安装在Python环境中
3. 本地修改会自动反映在环境中

示例验证方法:
1. 检查安装:
```bash
pip list | grep 包名
pip show 包名
```
2. 验证可编辑状态:
```bash
python -c "import 包名; print(包名.__file__)"  # 应显示本地路径
```

#### `uv help` - 获取帮助
```bash
# 显示通用帮助
uv help
# 显示特定命令帮助
uv help add
```

## 配置指南

### 在.bashrc中添加配置
编辑用户主目录下的.bashrc文件，添加以下内容：

```bash
# uv配置python下载源和pip源
# Python 解释器安装源 (清华镜像，代替 GitHub)
export UV_PYTHON_INSTALL_MIRROR="https://github.com/indygreg/python-build-standalone/releases/download"
# Python 包索引源 (阿里云，比清华快)
export UV_DEFAULT_INDEX="https://mirrors.aliyun.com/pypi/simple/"
```

保存后执行：
```bash
source ~/.bashrc
```

## 高级用法
- 使用`uv --help`查看所有可用命令
## PyTorch CUDA 配置

对于需要使用CUDA加速的PyTorch安装，请在`pyproject.toml`中添加如下配置:

```toml
[tool.uv.index]
name = "pytorch-cu126"
url = "https://download.pytorch.org/whl/cu126"
explicit = true

[tool.uv.sources]
torch = [
  { index = "pytorch-cu126", marker = "platform_system != 'Darwin'" },
]
torchvision = [
  { index = "pytorch-cu126", marker = "platform_system != 'Darwin'" },
]
```

说明:
- pytorch官方关于uv的使用
- https://hellowac.github.io/uv-zh-cn/guides/integration/pytorch/#_1 
- `pytorch-cu126`: 指定CUDA 12.6版本的PyTorch索引
- `pytorch-cu126`: 指定CUDA 11.8版本的索引(非macOS系统)
- `marker`: 条件表达式，排除macOS系统
### 安装使用说明

1. **配置后安装**:
```bash
# 配置好pyproject.toml后可直接安装(会自动使用配置的索引源)
uv add torch torchvision
```

2. **手动指定安装源**(临时覆盖配置):
```bash
uv add torch --index-url https://download.pytorch.org/whl/cu126
uv add torchvision --index-url https://download.pytorch.org/whl/cu126
```

4. **验证安装效果**:
```python
import torch
print(torch.__version__)
print(torch.cuda.is_available())
print(torch.cuda.get_device_name(0)) 
```


---

- 使用`uv 命令 --help`查看特定命令的帮助

