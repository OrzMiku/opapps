# opapps

自用的 1panel 本地应用仓库。

## 与其他本地应用仓库一起使用

1. 在 1Panel 控制面板中，进入"计划任务"页面。
2. 点击"新增任务"，选择任务类型为"Shell 脚本"。
3. 在脚本框中粘贴以下代码：

```bash
#!/bin/bash

# 任何命令执行失败，则立即退出脚本
set -e

# --- 配置路径 ---
# 1Panel 本地应用最终目录
FINAL_APP_DIR="/opt/1panel/resource/apps/local"
# 创建一个唯一的临时暂存目录，用于合并所有应用源
STAGING_DIR=$(mktemp -d /tmp/appstore_build.XXXXXX)
# 创建一个唯一的临时目录，用于克隆 Git 仓库
CLONE_DIR=$(mktemp -d /tmp/appstore_clones.XXXXXX)


# --- 自动清理函数 ---
# 使用 trap 命令，确保脚本在任何情况下退出时（成功、失败、中断），都会执行 cleanup 函数
cleanup() {
  echo "正在执行清理操作，删除临时目录..."
  rm -rf "$STAGING_DIR"
  rm -rf "$CLONE_DIR"
  echo "临时目录已清理。"
}
trap cleanup EXIT


# --- 脚本主逻辑 ---
echo "脚本开始执行，准备合并多个应用商店源..."
echo "暂存目录: $STAGING_DIR"
echo "克隆目录: $CLONE_DIR"

# 确保暂存目录存在
mkdir -p "$STAGING_DIR"

# 1. 处理 okxlin/appstore
echo "[1/4] 正在处理 okxlin/appstore..."
git clone -b localApps --depth=1 https://github.com/okxlin/appstore "$CLONE_DIR/okxlin"
# 将 app 内容复制到暂存目录，注意路径末尾的 `/.` 表示复制目录内的所有内容
cp -a "$CLONE_DIR/okxlin/apps/." "$STAGING_DIR/"

# 2. 处理 arch3rPro/1Panel-Appstore
echo "[2/4] 正在处理 arch3rPro/1Panel-Appstore..."
git clone --depth=1 https://github.com/arch3rPro/1Panel-Appstore "$CLONE_DIR/arch3rPro"
cp -a "$CLONE_DIR/arch3rPro/apps/." "$STAGING_DIR/"

# 3. 处理 opapps
echo "[3/4] 正在处理 opapps..."
git clone --depth=1 https://github.com/OrzMiku/opapps "$CLONE_DIR/opapps"
cp -a "$CLONE_DIR/opapps/apps/." "$STAGING_DIR/"

echo "所有应用源已成功合并到暂存目录。"

# 4. 同步到最终目录
echo "[4/4] 开始同步到最终应用目录..."
# 确保最终目标目录存在
mkdir -p "$FINAL_APP_DIR"
# 首先，清空最终目录下的所有旧内容。这是实现删除旧应用的关键。
# `/*` 和 `.` 配合可以安全地删除包括隐藏文件在内的所有内容
rm -rf "$FINAL_APP_DIR"/*
# 然后，将暂存目录中所有新的内容复制过去
# 使用 -a 标志可以保留文件权限等属性
cp -a "$STAGING_DIR"/. "$FINAL_APP_DIR/"

echo "应用商店数据已成功更新并同步！"
```
