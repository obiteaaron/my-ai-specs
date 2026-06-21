# AI 开发规范集合

本仓库收集了在 AI 辅助开发过程中，用于约束 AI 行为的各类规范文件。

## 文件说明

| 文件 | 用途 | 适用场景 |
|------|------|---------|
| `DOCS.md` | 文档管理规范 | 约束 AI 如何组织和维护项目文档，适用于所有开发者和 AI 工具 |
| `AGENTS.md` | AI Agent 通用行为规范 | 适用于 Cursor Agent、Codex、Windsurf 等各类 AI 编程助手 |
| `CLAUDE.md` | Claude Code 专用规范 | 专用于 Claude Code 的 `.claude/CLAUDE.md` |

## 使用方案

### 方案一：复制到项目仓库

将规范文件直接复制到目标项目中使用。

```bash
# 1. 复制文档管理规范到项目根目录（通用规范，不限于特定 AI 工具）
cp DOCS.md /path/to/your-project/DOCS.md

# 2. 复制 Agent 规范到项目根目录（适用于 Cursor/Codex 等）
cp AGENTS.md /path/to/your-project/AGENTS.md

# 3. 复制 Claude Code 规范到项目的 .claude/ 目录（Claude Code 专属）
cp CLAUDE.md /path/to/your-project/.claude/CLAUDE.md
```

复制后需修改文件中的「项目概述」部分，填入实际的项目名称和技术栈。

### 方案二：复制到 AI Agent 默认目录

将规范放到 AI 工具的全局配置目录中，使其对所有项目生效。

#### Claude Code（全局配置）

```bash
# Claude Code 的全局配置文件位于用户主目录
cp CLAUDE.md ~/.claude/CLAUDE.md
```

#### Cursor（全局规则）

Cursor 支持在 Settings > Rules 中配置全局规则，可将 AGENTS.md 内容粘贴到全局 Rules 中。

#### 其他 AI 工具

不同 AI 工具有各自的配置文件位置，请参考对应工具的文档将规范文件放到正确位置。

### 方案三：Git Submodule / 符号链接

如果多个项目共享同一套规范，可以使用 Git Submodule 或符号链接统一管理：

```bash
# 使用符号链接（推荐，实时同步）
ln -s "$(pwd)/CLAUDE.md" /path/to/your-project/.claude/CLAUDE.md
ln -s "$(pwd)/DOCS.md" /path/to/your-project/DOCS.md
ln -s "$(pwd)/AGENTS.md" /path/to/your-project/AGENTS.md

# 使用 Git Submodule
cd /path/to/your-project
git submodule add <this-repo-url> .ai-specs
cp .ai-specs/CLAUDE.md .claude/CLAUDE.md
```

## 自定义

每个文件中的「项目概述」部分需要根据实际项目修改：

```markdown
## 项目概述

项目名称：[替换为你的项目名称]
技术栈：[替换为你的技术栈描述]
```

其余规范内容可根据项目需要增删调整。
