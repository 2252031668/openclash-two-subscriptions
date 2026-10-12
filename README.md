# OpenClash 双订阅精简配置

目标：大陆和内网直连，普通海外流量使用一分机场，OpenAI/ChatGPT 只使用家宽订阅中的美国节点。

## 文件

- `config/openclash-base.yaml`：不含订阅链接的 Mihomo 配置。
- `module/openclash-two-subscriptions.conf`：OpenClash 远程 YAML 覆写模块。

## 使用

1. 本仓库地址：<https://github.com/2252031668/openclash-two-subscriptions>。
2. 在 OpenClash「覆写设置 → 覆写模块」中新增 HTTP 模块，名称使用 `openclash_two_subscriptions`，适用配置选择全部，填入模块 Raw 地址：<https://raw.githubusercontent.com/2252031668/openclash-two-subscriptions/ac7536964b4742e16314ca0bf8b479e06b604ee0/module/openclash-two-subscriptions.conf>。
3. 当前模块适配 OpenClash 0.47.156：当前配置须设为 `/etc/openclash/config/openclash-base.yaml`；模块负责下载模板并在运行配置中写入订阅。
4. 设置模块变量：

```text
EN_KEY1=一分机场订阅链接
EN_KEY2=家宽订阅链接
```

订阅地址只保存在路由器本地，不要提交到仓库。

## 说明

- 两个 Provider 每 48 小时更新一次。
- 一分机场自动组固定匹配 14 个节点：台湾 1 个、香港 2 个、新加坡 2 个、美国 4 个、日本 5 个。每 3 分钟检测一次，切换容差为 100ms；订阅方更改节点名称时需同步更新匹配名单。
- 外层 `🚀 一分机场` 默认选择自动组；手动选择具体节点后会固定使用该节点，选回 `🚀 一分机场自动` 即恢复自动选择。建议在外层组手选节点，保持内层自动组自动运行。
- AI 自动组只匹配当前家宽配置中的 5 个美国候选节点，每 5 分钟一次。
- 两个自动组均使用 `lazy: false` 持续检测；失效节点会被排除，新连接使用当前可用节点。已经断开的连接需要应用重连。
- `172.16.8.222` 命中 `172.16.0.0/12`，走 `DIRECT`。
- 测速地址 `https://www.gstatic.com/generate_204` 只检查连通性和响应延迟，不保证 OpenAI 解锁；首次使用需在 OpenClash 中逐个确认 AI 节点。

如果订阅方修改了节点名称，需要同步更新对应自动组的 `filter` 正则。
