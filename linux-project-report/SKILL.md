---
name: linux-project-report
description: Use when Codex needs to draft, complete, refine, or export Chinese Linux project reports, Linux automation operations reports, service deployment reports, testing reports, or application publishing reports. Trigger for tasks involving Linux labs or projects such as LAMP, LNMP, Nginx, Apache, DNS, MariaDB/MySQL, PHP, Docker, Shell automation, monitoring, web publishing, network service setup, command-heavy evidence, screenshots, teacher-provided report templates, or teacher-provided PDF project guides.
---

# Linux Project Report

## Workflow

1. Read the user's task brief, screenshots, commands, configuration files, and any existing draft. When provided, read the teacher PDF project guide and teacher template first.
2. Identify the required report sections from the current task. Use the complete structure below as the default framework, but remove sections that the teacher's current task does not require.
3. Before drafting, list only the missing personalized or environment-specific information that is needed. Ask for missing values instead of inventing them.
4. Draft in Chinese with a formal student project-report tone. Keep commands and configuration content reproducible.
5. Check the final report for placeholders, figure/table numbering, test conclusions, and consistency with the task.
6. If the user asks for a Word document, combine this skill with a DOCX/Word report workflow and visually verify the rendered document when possible.

## Teacher PDF Project Guides

When the user provides a teacher-issued PDF project guide or experiment instruction book, treat it as the primary source for the current project. Extract the required tasks, report structure, service configuration requirements, evidence requirements, test items, and submission naming rules from the PDF before relying on the default framework.

If no teacher PDF guide or template is available, proceed with the default report framework, the user's explicit requirements, and sound implementation judgment. Do not block on a missing guide.

Do not blindly copy personalized examples from the guide. Names, account names, hostnames, domains, paths, and identifiers such as `孙清闻`, `sunqingwen`, student IDs, class names, or teacher-specific sample values may be placeholders or examples from another student. Treat them as candidate examples unless the user confirms they are the user's own values.

If the PDF contains personalized-looking values that are required for the report or configuration, ask for the user's actual value or use a neutral placeholder only when the user wants a draft. Record unresolved personalized fields in the missing-information checklist.

If the PDF conflicts with the user's latest explicit instruction, follow the user's latest instruction and note the conflict if it affects the report or deployment steps.

## Default Report Framework

Use this complete framework unless the teacher provides a shorter structure:

```markdown
# 项目名称

## 1. 项目设计
### 1.1 总体方案
### 1.2 架构设计
### 1.3 服务基础规划

## 2. 项目实施
### 2.1 子任务或服务配置
### 2.2 子任务或服务配置
### 2.3 子任务或服务配置

## 3. 项目测试
### 3.1 测试项
### 3.2 测试项

## 问题与解决
### 4.1 故障现象/复现
### 4.2 排查过程
### 4.3 解决方案
### 4.4 经验教训
```

Adapt subsection names to the actual project, for example `数据库服务器配置`, `Web服务器配置`, `DNS解析配置`, `应用安装向导`, `远程连接测试`, or `内网客户端访问测试`.

## Information Collection

Before generating the final report, collect missing values in a compact checklist. Ask only for fields that are not available from the user's materials.

Common personalized fields:

- 姓名、学号、姓名全拼、学号后三位、班级
- 项目名称、任务编号、课程名称、指导教师
- 服务器角色、主机名、IP地址、操作系统版本、软件包名称和版本
- 域名、站点根目录、配置文件路径、数据库名、数据库用户名、数据库密码
- 管理员用户名、管理员密码、测试账号、访问地址
- 截图或截图说明、命令输出、配置文件内容、测试现象、故障现象

For passwords or other sensitive values, use the real value when the user explicitly provides it. Do not invent passwords, accounts, IP addresses, hostnames, or student identity information.

When the source template filename contains placeholders such as `学号-姓名-项目名称.docx`, update the output filename with the provided student ID and name. Keep the original task title portion unless the user asks to rename it. Example: `2024123456-张三-LVM测试报告.docx`.

Use `Rocky Linux` as the default Linux distribution in reports unless the user provides a more specific version or the teacher's task requires a different distribution.

Do not add student information, class information, experiment environment, or other front-matter fields into the report body unless the teacher template explicitly contains those fields or the user asks to include them. Student ID and name may still be used to rename the output file when provided.

## Writing Rules

- Do not leave teacher-template hints, blue placeholder text, square-bracket instructions, or `.....` in the final report.
- Use concise Chinese captions for every figure and table. Number figures and tables continuously from the first occurrence.
- Every test or evidence item in the teacher template should have its own corresponding image, screenshot, or clearly labeled image placeholder. Do not use one image as the only evidence for multiple later test items unless the user explicitly says the same screenshot covers them.
- Prefer tables for host planning, service planning, software planning, and troubleshooting.
- Put commands, SQL statements, and configuration files in fenced code blocks.
- Include enough command output, configuration content, or screenshot description to prove the result.
- For each test item, state the operation, observed result, and conclusion.
- Write test conclusions in the template style, for example: `由图可知，域名解析正常，客户端能够访问目标服务。`
- Keep the project design section explanatory, the implementation section procedural, and the testing section evidence-based.
- Do not over-explain generic Linux theory unless the teacher's task asks for it.

## Tables And Figures

Use this host planning table when the task involves multiple machines or service roles:

```markdown
表1 服务器主机表

| 主机角色 | 主机名 | IP地址 | 所需安装的软件包 |
|---|---|---|---|
| Web服务器 |  |  |  |
| 数据库服务器 |  |  |  |
```

For screenshots, use a short placeholder only when the actual image is not available:

```markdown
[图1 LAMP平台架构图：展示客户端、Web服务器、数据库服务器之间的访问关系]
```

When the user provides screenshots, insert the image or describe its verified evidence according to the output format being created.

## Section Guidance

`项目设计` should describe the architecture, server division of labor, domain or network design, and service plan. If the task is small, keep only the relevant planning paragraphs.

`项目实施` should follow the actual build order. Include installation commands, key configuration files, database or user creation statements, service startup commands, and verification commands.

`项目测试` should prove the project works from the client or service perspective. Include access tests, parsing tests, database connection tests, service status checks, or function tests as appropriate.

`问题与解决` should be included when the user has troubleshooting material or the teacher template requires it. If no fault occurred, write a short realistic summary such as `本次实施过程中未出现影响最终结果的故障，主要需要注意配置文件路径、服务重启顺序和防火墙放行状态。`

## Final Quality Check

Before delivery, verify:

- The report matches the teacher's current task, not only the complete reference template.
- Required sections are present and unnecessary sections are removed.
- Personalized fields are filled from user-provided information or explicitly listed as missing.
- Figure and table numbers are continuous and captions are specific.
- Commands, SQL, and configuration files are formatted clearly.
- Test conclusions are explicit.
- No placeholder instructions, brackets, blue-text wording, or `.....` remain.
- Remove extra blank paragraphs and trailing empty pages before delivery.
