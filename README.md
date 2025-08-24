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

# 特殊源安装(CUDA相关包)
uv add torch torchvision \
  --default-index https://download.pytorch.org/whl/cu118
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
- 使用`uv 命令 --help`查看特定命令的帮助
