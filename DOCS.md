# 文档管理规范

## 文档目录结构

```
docs/
├── index.html                 # GitHub Pages 入口（必须在根目录）
├── favicon.svg                # GitHub Pages 图标（必须在根目录）
├── plans/                    # 计划文件（待实现的设计方案）
├── specs/                    # 规格文件（功能规格、接口定义）
├── completions/              # 已完成的文档（从 plans 移入）
├── superpowers/              # Superpowers 技能相关文档
│   ├── plans/                # Superpowers 计划文件
│   ├── specs/                # Superpowers 规格文件
│   └── completions/          # Superpowers 完成文档（从 plans 移入）
└── <其他文档>                # 通用技术文档（分析、规则等）
```

## GitHub Pages 文件

`docs/index.html` 和 `docs/favicon.svg` 是 GitHub Pages 入口文件。
由于 GitHub Pages 只支持一级目录配置，这些文件**必须放在 docs/ 根目录**才能正常部署。

## 核心原则

**状态由物理位置体现，不使用索引文件。**

- `plans/` 中的文件 = 待实现或进行中
- `completions/` 中的文件 = 已完成

## 文件命名规范

### 基本格式
所有文档文件必须以日期开头，格式为 `yyyy-MM-dd-<名称>.md`：

```
2026-06-20-name-sync-analysis.md
2026-06-19-electron-desktop-app.md
```

### 日期确定规则
- **新文档**：使用创建当天的日期
- **旧文档整理**：通过 git 提交记录确认首次提交日期
  ```bash
  git log --format="%ad" --date=short --follow -- docs/<文件名> | tail -1
  ```

### 名称命名规则
- 使用英文小写字母、数字和连字符
- 简洁描述文档主题
- 避免使用特殊字符和空格

## 目录用途说明

### `docs/plans/`
存放计划性文档，包括：
- 功能设计方案
- 技术改进计划
- 模块重构规划
- 待实现的架构设计

**状态标识**：文件在此目录 = 计划待实现或正在开发中。

### `docs/specs/`
存放规格文档，包括：
- 功能行为规格
- API 接口定义
- 数据模型规格
- 测试规格与验收标准

**说明**：规格文档定义"是什么"，完成后仍保留在此目录作为参考，**不移动**。

### `docs/completions/`
存放已完成的文档，**从 `plans/` 移入**：
- 实现总结报告
- 技术决策记录
- 问题解决方案文档

**状态标识**：文件在此目录 = 功能已实现并验证通过。

### `docs/superpowers/`
与 `docs/` 根目录结构一致：
- `plans/` - 技能计划文件，完成后移至 `completions/`
- `specs/` - 技能规格文件，不移动
- `completions/` - 已完成的技能文档

## 文档状态流转

```
plans/ → 实现完成 → git mv → completions/
```

### 流转流程

1. **创建计划**：在 `plans/` 创建计划文档
2. **开发实现**：按计划开发功能
3. **完成验证**：通过代码审查和测试验证
4. **移动文档**：使用 `git mv` 将文档移至 `completions/`

### 移动命令

```bash
# 单文件移动
git mv docs/plans/2026-06-19-xxx-plan.md docs/completions/

# Superpowers 文档移动
git mv docs/superpowers/plans/2026-06-19-xxx.md docs/superpowers/completions/
```

### 流转条件

必须满足以下条件才能移动：
1. 对应功能已开发完成
2. 通过代码审查
3. 通过测试验证
4. 文档内容更新为完成总结（可选）

### 不移动的文档

以下文档完成后**不移动**，保留在原目录：
- `specs/` 中的规格文档（仍有参考价值）
- `docs/` 根目录的分析、规则文档（非计划性质）

## 文档分类指南

| 文档类型 | 存放位置 | 完成后处理 |
|---------|---------|-----------|
| 功能设计计划 | `plans/` | 移至 `completions/` |
| 功能规格定义 | `specs/` | 不移动 |
| 技术分析文档 | `docs/` 根目录 | 不移动 |
| 规则/标准文档 | `docs/` 根目录 | 不移动 |
| Superpowers 计划 | `superpowers/plans/` | 移至 `superpowers/completions/` |
| Superpowers 规格 | `superpowers/specs/` | 不移动 |

## 非文档文件

以下文件不属于文档管理范畴，但可存放在 `docs/` 目录中：
- `*.html` - 预览/测试页面（除 GitHub Pages 入口外）
- `*.svg` - 图形资源
- `*.json` - 配置文件
- 其他资源文件

**存放规则**：放在引用它的源文档同目录下。

示例：
```
docs/superpowers/specs/
├── 2026-06-19-landing-page-design.md
└── landing-preview.html              # 被引用的预览页面
```

**命名规范**：非文档文件不受日期命名约束。

**例外**：GitHub Pages 入口文件必须在 docs/ 根目录，见上方说明。

## 旧文档整理规范

### 整理范围

默认扫描仓库下所有 `.md` 文件，但以下目录自动排除：
- `.git/`
- `node_modules/`
- `.claude/`
- 其他配置目录

### 整理流程

**必须先确认范围，再执行整理。**

1. **扫描文件**：列出所有待整理的 markdown 文件
2. **用户确认**：
   - 确认整理范围
   - 可增加其他文件类型（如 `.txt`）
   - 可排除特定文件夹或文件
3. **确定日期**：通过 git 提交记录获取首次提交日期
   ```bash
   git log --format="%ad" --date=short --follow -- <文件路径> | tail -1
   ```
4. **执行重命名**：将文件重命名为 `yyyy-MM-dd-<原名>.md`
   ```bash
   git mv <原文件> <新文件>
   ```

### 整理命令模板

```bash
# 扫描待整理文件（排除已符合规范的）
find . -name "*.md" -not -path "./.git/*" -not -path "./node_modules/*" \
  -not -name "^[0-9]{4}-[0-9]{2}-[0-9]{2}-.*\.md$" | sort

# 获取文件首次提交日期
git log --format="%ad" --date=short --follow -- <文件> | tail -1

# 重命名文件
git mv <原文件> <yyyy-MM-dd-原文件>
```

### 用户确认内容

执行整理前需向用户展示：
- 待整理文件列表
- 每个文件的首次提交日期
- 建议的新文件名

用户可选择：
- ✓ 确认全部整理
- ✓ 增加其他文件类型
- ✓ 排除特定文件/目录
- ✓ 取消整理
