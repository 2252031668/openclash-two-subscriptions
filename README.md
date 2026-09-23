# OpenClash 双订阅精简配置

目标：大陆和内网直连，普通海外流量使用一分机场，OpenAI/ChatGPT 只使用家宽订阅中的美国节点。

## 文件

- `config/openclash-base.yaml`：不含订阅链接的 Mihomo 配置。
- `module/openclash-two-subscriptions.conf`：OpenClash 远程 YAML 覆写模块。

## 使用

1. 本仓库地址：<https://github.com/2252031668/openclash-two-subscriptions>。
2. 在 OpenClash「覆写设置 → 覆写模块」中新增 HTTP 模块，填入模块 Raw 地址：<https://raw.githubusercontent.com/2252031668/openclash-two-subscriptions/47aff3b21cfc8db99841a844efd29136ba725b1c/module/openclash-two-subscriptions.conf>。
4. 设置模块变量：

```text
EN_KEY1=一分机场订阅链接
EN_KEY2=家宽订阅链接
```

订阅地址只保存在路由器本地，不要提交到仓库。

## 说明

- 两个 Provider 每 48 小时更新一次。
- 一分机场自动组只测速当前订阅中精选的 12 个节点，每 30 分钟一次。
- AI 自动组只匹配当前家宽配置中的 5 个美国候选节点，每 30 分钟一次。
- `172.16.8.222` 命中 `172.16.0.0/12`，走 `DIRECT`。
- 测速地址 `https://www.gstatic.com/generate_204` 只检查连通性和响应延迟，不保证 OpenAI 解锁；首次使用需在 OpenClash 中逐个确认 AI 节点。

如果订阅方修改了节点名称，需要同步更新基础 YAML 中两个 `filter` 正则。
