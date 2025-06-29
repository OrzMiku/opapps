# opapps

自用的 1panel 本地应用仓库。

## 使用方法

### 添加脚本到 1Panel 计划任务

1. 在 1Panel 控制面板中，进入"计划任务"页面。
2. 点击"新增任务"，选择任务类型为"Shell 脚本"。
3. 在脚本框中粘贴以下代码：

```bash
#!/bin/bash

# 清理旧的临时目录
rm -rf /tmp/appstore_merge

# 克隆 opapps
git clone --depth=1 https://ghfast.top/https://github.com/OrzMiku/opapps /tmp/appstore_merge/opapps

# 复制数据
cp -rf /tmp/appstore_merge/appstore-opapps/apps/* /opt/1panel/resource/apps/local/

# 清理临时目录
rm -rf /tmp/appstore_merge
echo "应用商店数据已更新"
```
