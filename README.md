<h1 align="center">Grok Mirror</h1>

[![Docker](https://img.shields.io/docker/pulls/dairoot/grok-gateway?label=Grok-Mirror&logo=docker)](https://hub.docker.com/r/dairoot/grok-gateway)
[![License](https://img.shields.io/github/license/dairoot/Grok-Mirror)](./LICENSE)

Grok Mirror 后台是一个 Grok 镜像站，允许多账号共享管理。实现多人同时使用 Grok 服务。

## 特点

- 提供与官网同等的极致体验。
- 提供 聊天 API `/v1/chat/completions` (格式同 Openai)
- 用户无需翻墙，便可轻松访问并使用 Grok 官方网站的所有功能。
- 通过在 `Mirror` 后台录入 `Sso Token`，让用户可以轻松使用 Grok 服务。

## 在线体验

https://gk.dairoot.cn

## 部署

打开命令行终端，执行以下命令

```bash
docker run --rm -p 50005:8080 \
-v grok_gateway_db:/app/.cache_data \
dairoot/grok-gateway:latest
```

访问 `http://127.0.0.1:50005` 或访问 `http://外网ip:50005`

配置域名，请点击查看 [完整部署流程](./docs/deploy.md)

## 环境变量

<table>
  <tr align="left">
    <th>分类</th>
    <th>变量名</th>
    <th>类型</th>
    <th>默认值</th>
    <th>描述</th>
  </tr>
  <tr align="left">
    <td rowspan="5">API 相关</td>
    <td><code>ADMIN_PASSWORD</code></td>
    <td><code>String</code></td>
    <td><code>None</code></td>
    <td>管理后台访问密码</td>
  </tr>
  <tr align="left">
    <td><code>AUTHORIZATION</code></td>
    <td><code>String</code></td>
    <td><code>None</code></td>
    <td>用于轮询 多账号 SsoToken 列表，的API访问密钥</td>
  </tr>
  <tr align="left">
    <td><code>ENABLE_MIRROR_API</code></td>
    <td><code>Boolean</code></td>
    <td><code>true</code></td>
    <td>是否开启 API 访问</td>
  </tr>
  <tr align="left">
    <td><code>MIRROR_API_PREFIX</code></td>
    <td><code>String</code></td>
    <td><code>None</code></td>
    <td>API 访问前缀，建议配置</td>
  </tr>
  <tr align="left">
    <td><code>API_HATD</code></td>
    <td><code>Boolean</code></td>
    <td><code>true</code></td>
    <td>API 开启临时聊天（不保存聊天记录）</td>
  </tr>
   <tr align="left">
    <td rowspan="4">系统变量</td>
    <td><code>PROXY_URL_POOL</code></td>
    <td><code>String</code></td>
    <td><code>None</code></td>
    <td>代理池链接，多个代理用逗号分隔<br><code>http://username:password@ip:port,</code><br/><code>socks5://username:password@ip:port,</code><br/><code>socks5h://username:password@ip:port</code></td>
   </tr>
   <tr align="left">
    <td><code>HOST</code></td>
    <td><code>String</code></td>
    <td><code>None</code></td>
    <td>设置服务的域名地址，用于api 图片显示 <br><code>https://example.com</code></td>
    </tr>
     <tr align="left">
    <td><code>GOOGLEADS</code></td>
    <td><code>String</code></td>
    <td><code>None</code></td>
    <td>Google Adsense 广告代码 </code></td>
  </tr>
  </tr>
  
</table>

## 聊天 API 接口

避免滥用限制每日200次

POST: /v1/chat/completions

- 请求头:

| 字段            | 类型     | 默认值 | 必填 | 描述                                                                                                                                                |
| --------------- | -------- | ------ | ---- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Authorization` | `string` | `None` | `是` | `Bearer ${ AUTHORIZATION }` 或 <br> `Bearer ${`[Sso Token](./docs/get-sso-token.jpg)`}` <br><br> 推荐使用环境变量中的 AUTHORIZATION，自动轮询 token |

- 请求参数:

| 参数名            | 类型    | 是否必须 | 描述                                                               |
| ----------------- | ------- | -------- | ------------------------------------------------------------------ |
| model             | string  | 是       | 模型名称 <br> `grok-3` `grok-4` `grok-4-thinking` |
| messages          | array   | 是       | 消息内容                                                           |
| stream            | boolean | 否       | 是否开启流式返回                                                   |

聊天接口请求示例：

```bash
export Authorization=GrokToken 或者 环境变量的Authorization
export yourUrl=http://127.0.0.1:50005


curl --location "${yourUrl}/v1/chat/completions" \
--header 'Content-Type: application/json' \
--header "Authorization: Bearer ${Authorization}" \
--data '{
     "stream": true,
     "model": "grok-2",
     "messages": [{"role": "user", "content": "你好呀!"}]
   }'
```

更多 API 请点击查看：[高阶玩法](./docs/grok-gateway.md)

## 加入群聊

[Telegram](https://t.me/+34aYksZdq5ZhMzhl)

## 捐赠

[Buy Me a Coffee](https://github.com/dairoot/ChatGPT-Mirror/blob/main/docs/donation.md)

## Star History

![Star History Chart](https://api.star-history.com/svg?repos=dairoot/Grok-Mirror&type=Timeline)
