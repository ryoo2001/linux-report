# 自动化报告撰写项目规则

本项目用于根据老师要求自动生成中文 Linux 实验/项目报告，并配合 SSH MCP 与截图脚本采集证据。

## 默认环境

- 默认使用测试项目主机表。
- 只有当用户明确说明“正式项目”时，才切换到正式项目主机表或正式项目证据目录。
- 未提供正式项目主机表前，不要猜测正式项目 IP、账号、密码或服务角色。

## 测试项目主机表

| 主机名 | SSH 管理地址 | SSH 端口 | 用户 | 密码 | 内网 IP |
|---|---:|---:|---|---|---:|
| practice01 | `<SSH_HOST_1>` | 22 | `<SSH_USER>` | `<SSH_PASSWORD>` | `<PRIVATE_IP_1>` |
| practice02 | `<SSH_HOST_2>` | 22 | `<SSH_USER>` | `<SSH_PASSWORD>` | `<PRIVATE_IP_2>` |
| practice03 | `<SSH_HOST_3>` | 22 | `<SSH_USER>` | `<SSH_PASSWORD>` | `<PRIVATE_IP_3>` |
| practice04 | `<SSH_HOST_4>` | 22 | `<SSH_USER>` | `<SSH_PASSWORD>` | `<PRIVATE_IP_4>` |

## SSH MCP 规则

- 项目级 `.codex/config.toml` 可按 `.codex/config.example.toml` 配置 4 个 SSH MCP server：`practice01`、`practice02`、`practice03`、`practice04`。
- 需要远程执行命令时，优先使用对应主机名的 MCP 工具。
- 如果测试主机使用特权用户登录，并且 MCP 配置禁用了 `sudo` 工具，需要提权命令时直接用普通 `exec` 执行。
- 报告正文中的拓扑、访问测试和服务规划优先使用内网 IP；SSH 管理地址只用于自动化连接和证据采集说明。

## Skill 工作流

- 涉及 Linux 项目报告、实验报告、服务部署报告、命令证据或截图时，优先使用 `linux-project-report` skill。
- 报告生成顺序默认是：读取任务要求 -> 确认缺失信息 -> SSH MCP 采集命令输出 -> 截图脚本生成证据图片 -> 撰写报告正文 -> 检查图号、表号和结论。
- 不要在报告中编造学号、姓名、密码、IP、域名、配置文件路径或命令输出。缺失时先列为待补信息。

## 截图规则

- 默认证据目录：`evidence/<任务名或日期>/`。
- 图片命名：`figNN_<host>_<kind>_<slug>.png`，例如 `fig01_practice01_terminal_hostname-i.png`。
- 每次生成截图时同步维护 `manifest.json`，记录图号、主机、命令或 URL、图片路径和中文 caption。
- 网页能从本机访问时使用网页截图；内网服务测试或命令验证先通过 SSH MCP 获取输出，再生成终端风格截图。
- 报告引用截图时，图号应与 `manifest.json` 保持一致，并写出“操作、现象、结论”。
