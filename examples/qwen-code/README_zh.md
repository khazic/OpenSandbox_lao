# Qwen Code 示例

在 OpenSandbox 沙箱容器中运行 [Qwen Code](https://github.com/QwenLM/qwen-code)，并通过 OpenAI-compatible 接口访问模型服务。

## 启动 OpenSandbox Server（本地）

预先拉取 `code-interpreter` 镜像（内置 Node.js）：

```shell
docker pull sandbox-registry.cn-zhangjiakou.cr.aliyuncs.com/opensandbox/code-interpreter:v1.0.2

# 使用 Docker Hub
# docker pull opensandbox/code-interpreter:v1.0.2
```

然后启动本地 OpenSandbox 服务，日志会持续输出在当前终端：

```shell
uv pip install opensandbox-server
opensandbox-server init-config ~/.sandbox.toml --example docker
opensandbox-server
```

## 创建并访问 Qwen Sandbox

```shell
# 安装 OpenSandbox 包
uv pip install opensandbox

# 导出模型服务配置
export API_KEY=your-api-key
export BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
export MODEL_NAME=qwen3-coder-plus

# 运行示例
uv run python examples/qwen-code/main.py
```

该脚本会在运行时安装 Qwen Code（`npm install -g @qwen-code/qwen-code@latest`），然后在沙箱内部生成项目级 `.qwen/settings.json`，并以 headless 模式执行 `qwen -p "Compute 1+1 and reply with only the final number."`。API Key 只通过 `API_KEY` 环境变量注入，不会写入仓库代码。

## 环境变量

- `SANDBOX_DOMAIN`：OpenSandbox 服务地址（默认：`localhost:8080`）
- `SANDBOX_API_KEY`：如果服务端开启鉴权，则需要传入 API Key（本地默认可选）
- `SANDBOX_IMAGE`：要使用的沙箱镜像（默认：`sandbox-registry.cn-zhangjiakou.cr.aliyuncs.com/opensandbox/code-interpreter:v1.0.2`）
- `API_KEY`：Qwen Code 使用的 OpenAI-compatible 服务 API Key（必填）
- `BASE_URL`：OpenAI-compatible 接口地址（默认：`https://dashscope.aliyuncs.com/compatible-mode/v1`）
- `MODEL_NAME`：模型名称（默认：`qwen3-coder-plus`）

## 参考

- [Qwen Code](https://github.com/QwenLM/qwen-code) - 官方仓库
- [Qwen Code Authentication](https://qwenlm.github.io/qwen-code-docs/en/users/configuration/auth/) - Provider 配置说明
