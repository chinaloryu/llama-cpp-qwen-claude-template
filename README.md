# llama-cpp-qwen-claude-template

A Jinja2 chat template that makes **Qwen 3.8 27B** on **llama.cpp** speak the **OpenAI Chat completions ** message format via CC Switch proxy.

让 Qwen 3.8 27B 在 llama.cpp 上以 OpenAI Chat completions 的消息格式与CC Switch网关进行对话的聊天模板。

## Features / 特性

- OpenAI-style tool calling: `tool_use` / `tool_result` blocks with `antml:invoke` / `antml:function` / `antml:parameter` XML
- Thinking / reasoning support: `reasoning_content` round-trip, `enable_thinking` toggle, `reasoning_effort` levels
- `developer` role support; multiple leading system/developer messages are merged into one
- Vision placeholders for image / video content, with optional `Picture N:` / `Video N:` numbering
- Multi-step tool-call conversations handled correctly (`preserve_thinking` aware)
- 工具调用采用 OpenAI 风格（`tool_use` / `tool_result` + `antml:*` XML）
- 支持思考模式（`reasoning_content` 往返、`enable_thinking` 开关、`reasoning_effort` 档位）
- 支持 `developer` 角色，多条前置 system/developer 消息自动合并
- 图像 / 视频占位符，可选 `Picture N:` / `Video N:` 编号

## Usage / 用法

Point llama.cpp's server at the template:

```bash
llama-server \
  -m /path/to/qwen3.8-27b-*.gguf \
  --chat-template qwen3.8-27b-llama-cpp-claude-compatible.jinja \
  --port 8080
```

Then point any Claude/OpenAI-compatible client at `http://localhost:8080`.

在 llama.cpp 服务端指定该模板后，任何 Claude OpenAI 兼容的客户端都可以直接对接。

### Template variables / 模板变量

| Variable | Default | Description |
| --- | --- | --- |
| `enable_thinking` | `true` | Emit an open `antml:thinking` block in the generation prompt. Set `false` to emit an empty thinking block instead. |
| `reasoning_effort` | `xhigh` | `xhigh` (default), `medium`, or `low` (`high` is mapped to `xhigh`). Injects a matching instruction into the system message. |
| `preserve_thinking` | `true` | Keep `antml:thinking` blocks in assistant history. When `false`, thinking is dropped for turns before the last non-tool user message. |
| `add_vision_id` | — | Prefix vision placeholders with `Picture N:` / `Video N:`. |
| `add_generation_prompt` | — | Standard llama.cpp flag: append the assistant generation prompt. |

## License / 许可证

Released under the [MIT License](LICENSE).
