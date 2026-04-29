# Linux Report Workflow

这是一个用于辅助生成中文 Linux 实验/项目报告的 Codex 工作流仓库。它包含项目规则、报告撰写 skill，以及 SSH MCP 配置模板，适合用来根据老师任务要求、命令输出和截图证据整理实验报告。

## 目录结构

```text
.
├── AGENTS.md
├── .codex/
│   └── config.example.toml
├── linux-project-report/
│   ├── SKILL.md
│   └── agents/
│       └── openai.yaml
└── .gitignore
```

- `AGENTS.md`：项目级规则，描述默认环境、SSH MCP 使用方式、报告工作流和截图规范。
- `linux-project-report/SKILL.md`：Codex skill，定义中文 Linux 项目报告的撰写规则和质量检查要求。
- `linux-project-report/agents/openai.yaml`：skill 的界面与默认提示配置。
- `.codex/config.example.toml`：SSH MCP 示例配置，所有主机、账号和密码均已脱敏。

## 使用方式

1. 克隆仓库。

   ```bash
   git clone https://github.com/ryoo2001/linux-report.git
   cd linux-report
   ```

2. 准备本地 Codex 配置。

   将示例配置复制为真实配置：

   ```bash
   cp .codex/config.example.toml .codex/config.toml
   ```

   Windows PowerShell 可以使用：

   ```powershell
   Copy-Item .\.codex\config.example.toml .\.codex\config.toml
   ```

3. 修改 `.codex/config.toml`。

   把占位符替换成自己的 SSH 连接信息：

   ```text
   <SSH_HOST_1>
   <SSH_USER>
   <SSH_PASSWORD>
   <PRIVATE_IP_1>
   ```

   如果你的主机数量不是 4 台，可以按需删除或增加 `[mcp_servers.<name>]` 配置块。

4. 在 Codex 中打开这个仓库目录。

   Codex 会读取根目录的 `AGENTS.md` 作为项目规则。需要生成 Linux 报告时，可以明确要求使用 `linux-project-report` skill，例如：

   ```text
   使用 linux-project-report，根据老师任务要求和截图生成一份 Linux 实验报告。
   ```

## SSH MCP 配置说明

`.codex/config.example.toml` 使用 `ssh-mcp`，每台主机形如：

```toml
[mcp_servers.practice01]
command = "npx"
args = [
  "-y",
  "ssh-mcp",
  "--",
  "--host=<SSH_HOST_1>",
  "--port=22",
  "--user=<SSH_USER>",
  "--password=<SSH_PASSWORD>",
  "--disableSudo",
  "--timeout=120000",
  "--maxChars=none",
]
startup_timeout_sec = 20
tool_timeout_sec = 120
```

如果你的环境不需要禁用 sudo，可以按自己的需求调整 `--disableSudo`。如果使用密钥登录，也应按 `ssh-mcp` 支持的参数改成密钥方式，不要把私钥内容提交到仓库。

## 安全注意事项

- 不要提交真实的 `.codex/config.toml`。
- 不要提交 SSH 密码、私钥、token、真实公网 IP 或敏感内网拓扑。
- 不要把生成报告、截图、日志等临时输出直接提交，除非确认其中没有个人信息和敏感信息。
- 当前 `.gitignore` 已忽略：

  ```text
  .playwright/
  output/
  .codex/config.toml
  ```

## 报告工作流建议

推荐流程：

1. 读取老师任务说明、模板或 PDF。
2. 明确缺失的姓名、学号、主机规划、域名、账号等个性化信息。
3. 使用 SSH MCP 采集必要命令输出。
4. 生成或整理截图证据，并维护 `manifest.json`。
5. 按 `linux-project-report` 的结构撰写中文报告。
6. 检查图号、表号、测试结论、占位符和敏感信息。

## 维护建议

如果要新增实验类型或报告模板，优先修改 `linux-project-report/SKILL.md`。如果只是调整当前项目的默认主机、截图目录或执行偏好，优先修改 `AGENTS.md`，并继续保持脱敏。
