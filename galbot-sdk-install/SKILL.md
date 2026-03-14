---
name: galbot-sdk-install
description: 安装 Galbot SDK（.whl 包）并验证是否可以正常 import。当用户提到安装 galbot、galbot SDK、galbot whl、galbot python 包时触发。自动检测 Python 版本并选择对应的 wheel 文件。
---

# Galbot SDK 安装

## 概述

自动检测当前 Python 版本，选择对应的 galbot wheel 文件进行安装，并验证 import 是否成功。

## 步骤

### 1. 检测 Python 版本

```bash
python3 --version
```

根据输出选择对应的 whl 文件：
- Python 3.10 → `galbot-1.0.0+galbot.fixed-cp310-cp310-linux_x86_64.whl`
- Python 3.11 → `galbot-1.0.0+galbot.fixed-cp311-cp311-linux_x86_64.whl`
- Python 3.12 → `galbot-1.0.0+galbot.fixed-cp312-cp312-linux_x86_64.whl`

whl 文件默认路径：`/home/galbot/桌面/galbot_sdk/`

### 2. 安装

用实际版本号替换 `cp3XX`：

```bash
pip install /home/galbot/桌面/galbot_sdk/galbot-1.0.0+galbot.fixed-cp3XX-cp3XX-linux_x86_64.whl --force-reinstall --no-deps
```

### 3. 验证

```bash
python3 -c "import galbot; print('import galbot 成功'); print('模块路径:', galbot.__file__)"
```

输出 `import galbot 成功` 则安装完成。

## 完整命令参考

```bash
# Python 3.10
pip install /home/galbot/桌面/galbot_sdk/galbot-1.0.0+galbot.fixed-cp310-cp310-linux_x86_64.whl --force-reinstall --no-deps

# Python 3.11
pip install /home/galbot/桌面/galbot_sdk/galbot-1.0.0+galbot.fixed-cp311-cp311-linux_x86_64.whl --force-reinstall --no-deps

# Python 3.12
pip install /home/galbot/桌面/galbot_sdk/galbot-1.0.0+galbot.fixed-cp312-cp312-linux_x86_64.whl --force-reinstall --no-deps
```

## 注意事项

- `--no-deps` 避免覆盖已有依赖
- `--force-reinstall` 确保重新安装即使已存在旧版本
- 如果 pip 不在当前 conda 环境，先激活目标环境再安装
