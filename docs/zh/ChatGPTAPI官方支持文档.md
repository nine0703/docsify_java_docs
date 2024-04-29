## [介绍](https://platform.openai.com/docs/api-reference/introduction)

[您可以通过任何语言的 HTTP 请求、我们的官方 Python 绑定、我们的官方 Node.js 库或社区维护的库](https://platform.openai.com/docs/libraries/community-libraries)与 API 交互。

要安装官方 Python 绑定，请运行以下命令：

```bash
pip install openai
```

要安装官方 Node.js 库，请在 Node.js 项目目录中运行以下命令：

```bash
npm install openai
```

## [验证](https://platform.openai.com/docs/api-reference/authentication)

OpenAI API 使用 API 密钥进行身份验证。访问您的[API 密钥](https://platform.openai.com/account/api-keys)页面以检索您将在请求中使用的 API 密钥。

**请记住，您的 API 密钥是秘密的！**不要与他人共享或在任何客户端代码（浏览器、应用程序）中公开它。生产请求必须通过您自己的后端服务器进行路由，您的 API 密钥可以从环境变量或密钥管理服务中安全加载。

所有 API 请求都应在 HTTP 标头中包含您的 API 密钥，`Authorization`如下所示：

```bash
Authorization: Bearer YOUR_API_KEY
```

## [请求组织](https://platform.openai.com/docs/api-reference/requesting-organization)

对于属于多个组织的用户，您可以传递一个标头来指定哪个组织用于 API 请求。来自这些 API 请求的使用将计入指定组织的订阅配额。

卷曲命令示例：

```bash
curl https://api.openai.com/v1/models \
  -H 'Authorization: Bearer YOUR_API_KEY' \
  -H 'OpenAI-Organization: YOUR_ORG_ID'
```

Python 包的示例`openai`：

```python
import os
import openai
openai.organization = "YOUR_ORG_ID"
openai.api_key = os.getenv("OPENAI_API_KEY")
openai.Model.list()
```

Node.js 包的示例`openai`：

```javascript
import { Configuration, OpenAIApi } from "openai";
const configuration = new Configuration({
    organization: "YOUR_ORG_ID",
    apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);
const response = await openai.listEngines();
```

[组织 ID 可以在您的组织设置](https://platform.openai.com/account/org-settings)页面上找到。

## [发出请求](https://platform.openai.com/docs/api-reference/making-requests)

您可以将下面的命令粘贴到您的终端中以运行您的第一个 API 请求。确保替换`YOUR_API_KEY`为您的秘密 API 密钥。

```bash
curl https://api.openai.com/v1/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{"model": "text-davinci-003", "prompt": "Say this is a test", "temperature": 0, "max_tokens": 7}'
```

_此请求查询 Davinci 模型以完成以提示“ Say this is a test_ ”开头的文本。该参数设置了API 将返回的[令牌](https://platform.openai.com/tokenizer)`max_tokens`数量的上限。您应该会收到类似于以下内容的响应：[](https://platform.openai.com/tokenizer)

```json
{
    "id": "cmpl-GERzeJQ4lvqPk8SkZu4XMIuR",
    "object": "text_completion",
    "created": 1586839808,
    "model": "text-davinci:003",
    "choices": [
        {
            "text": "\n\nThis is indeed a test",
            "index": 0,
            "logprobs": null,
            "finish_reason": "length"
        }
    ],
    "usage": {
        "prompt_tokens": 5,
        "completion_tokens": 7,
        "total_tokens": 12
    }
}
```

现在你已经生成了你的第一个完成。`echo`如果您连接提示和完成文本（如果您将参数设置为 ，API 将为您执行此操作`true`），则生成的文本为“ _Say this is a test. This indeed a test._ ”您还可以将`stream`参数设置为`true`用于 API 流回文本（作为[仅数据服务器发送的事件](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events#Event_stream_format)）。

## [楷模](https://platform.openai.com/docs/api-reference/models)

列出并描述 API 中可用的各种模型。您可以参考[模型](https://platform.openai.com/docs/models)文档以了解可用的模型以及它们之间的区别。