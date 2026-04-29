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
   使用 linux-project-report，根据我提供的实验指导书、报告模板和服务器环境生成一份 Linux 实验报告。
   ```

5. 提供任务材料并启动工作流。

   建议一次性提交以下材料，缺少的内容可以后续补充：

   - 老师发布的实验指导书或项目任务书，如果有 PDF、Word、截图都可以提供。
   - 老师要求使用的报告模板，如果有。
   - 学号、姓名、班级、课程名等需要写入文件名或报告封面的个人信息。
   - 服务器规划、主机名、内网 IP、域名、数据库名等实验环境信息。
   - 已完成的命令输出、配置文件、网页截图或终端截图，如果已经有。

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


## 报告生成工作流

实际使用时，可以按下面的顺序推进：

1. 提交任务材料。

   先把实验指导书、项目任务书、报告模板交给 Codex。如果没有指导书或模板，也可以直接描述老师要求。

2. 确认缺失信息。

   Codex 会先检查材料里缺少哪些个性化或环境信息，例如姓名、学号、主机 IP、域名、数据库名、服务角色、截图要求等。缺失内容不应由 Codex 编造，应由使用者补充或明确允许使用占位符。

3. 启动报告工作流。

   明确让 Codex 使用 `linux-project-report` skill。工作流会根据老师要求确定报告结构，优先遵循实验指导书和报告模板，其次使用 skill 内置的默认报告框架。

4. 采集实验过程和证据。

   如果已配置 SSH MCP，Codex 可以通过对应主机采集命令输出，例如服务状态、配置文件、网络连通性、数据库测试结果等。需要网页或终端证据时，可以整理截图，并在 `manifest.json` 中记录图号、主机、命令或 URL、图片路径和中文说明。

5. 生成报告正文。

   Codex 会按照任务要求撰写中文报告，通常包括项目设计、项目实施、项目测试、问题与解决等部分。每个测试项应包含操作、现象和结论，截图编号和表格编号应连续。

6. 导出最终文档。

   根据老师要求生成最终文件，例如 `.docx` 报告。文件名优先按老师规则命名，例如“学号-姓名-项目名称.docx”。

7. 质量检查。

   生成后检查是否还有模板提示、占位符、错误图号、空白页、敏感信息、缺失截图或不完整结论。确认无误后再作为最终版本。

8. 确认是否上传作业系统。

   如果使用者要求，Codex 可以继续协助打开作业系统、选择课程任务、上传最终文档并核对提交状态。上传前应再次确认文件名、文件内容和目标任务是否正确。

## 维护建议

如果要新增实验类型或报告模板，优先修改 `linux-project-report/SKILL.md`。如果只是调整当前项目的默认主机、截图目录或执行偏好，优先修改 `AGENTS.md`，并继续保持脱敏。
