# Using iAManager with LM Studio (and other OpenAI-compatible servers)

iAManager is built around [Ollama](https://ollama.com) as its primary backend.
[LM Studio](https://lmstudio.ai), vLLM, oMLX and any other server that speaks
the OpenAI API can be connected as **external models**.

## What works

Once connected, an external model behaves like a local one in:

- Chat (including streaming, tool calling and vision when the model supports it)
- Web tools, Agent Mode, skills and MCP servers
- Benchmarks and the test panel
- The API gateway (the model can be re-exposed over OpenAI/Ollama protocols)

## What does not work

Model lifecycle management is Ollama-only. iAManager cannot download, delete,
repair, archive or unload models that live inside LM Studio. Use LM Studio's own
interface for that.

## Setup

1. In LM Studio, load a model and start the local server (Developer tab).
   The default address is `http://localhost:1234/v1`.
2. In iAManager, open the external models section and add a connection:
   - **Base URL**: `http://localhost:1234/v1`
   - **Model identifier**: the exact id LM Studio reports (`GET /v1/models`)
   - **API key**: any value, unless you enabled authentication in LM Studio
   - **Capabilities**: tick `tools`, `thinking` or `vision` if the model supports them
3. Use the **Test** button to verify connectivity and detect capabilities.
4. Enable the model; it now appears in the model list and the chat selector.

## Troubleshooting

- **Connection refused**: the LM Studio server is not running, or iAManager runs
  on another machine and LM Studio is bound to `127.0.0.1` only.
- **Model not found**: the identifier must match what `/v1/models` returns.
- **Tools unavailable**: capability tags are not auto-trusted; enable `tools`
  manually for models that support function calling.
