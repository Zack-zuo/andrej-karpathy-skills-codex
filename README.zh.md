# 受 Karpathy 启发的编码技能

一套可复用的技能与配套指令，面向 Claude Code、Cursor、Codex 以及其他类似的编码代理，源自 [Andrej Karpathy 对 LLM 编码陷阱的观察](https://x.com/karpathy/status/2015883857489522876)。

> 上游致谢：本仓库基于 [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) 整理与扩展。

[English](./README.md) | 简体中文

## 问题所在

来自 Andrej 的推文：

> "模型会代你做错误假设，然后不假思索地执行。它们不管理自身的困惑，不寻求澄清，不呈现矛盾，不展示权衡，在应该提出异议时也不反驳。"

> "它们真的很喜欢把代码和 API 搞复杂，堆砌抽象概念，不清理死代码……明明 100 行能搞定的事情，非要实现成 1000 行的臃肿架构。"

> "它们有时仍会改动或删除自己理解不足的代码和注释，即使这些内容与任务本身无关。"

## 解决方案

四个原则，集中在一个文件中，直接针对这些问题：

| 原则 | 解决什么问题 |
|-----------|-----------|
| **编码前思考** | 错误假设、隐藏困惑、缺少权衡 |
| **简洁优先** | 过度复杂、臃肿抽象 |
| **精准修改** | 无关编辑、触碰不应碰的代码 |
| **目标驱动执行** | 通过测试优先、可验证的成功标准 |

## 四个原则详解

### 1. 编码前思考

**不要假设。不要隐藏困惑。呈现权衡。**

LLM 经常默默选择一种解释然后直接执行。这个原则要求把推理显式化：

- **明确说明假设** - 如果不确定，先问，不要猜
- **给出多种解释** - 遇到歧义时，不要悄悄选一个
- **必要时提出异议** - 如果有更简单的办法，就说出来
- **困惑时停下来** - 说清楚哪里不明白，然后请求澄清

### 2. 简洁优先

**用最少的代码解决问题，不做任何猜测性扩展。**

对抗过度工程的倾向：

- 不要添加需求之外的功能
- 不要为一次性代码创建抽象
- 不要加入没有被要求的“灵活性”或“可配置性”
- 不要为不可能发生的场景写错误处理
- 如果 200 行能写成 50 行，就重写它

**检验标准：** 资深工程师会觉得这太复杂吗？如果会，就简化。

### 3. 精准修改

**只改必须改的，只清理自己制造的混乱。**

编辑现有代码时：

- 不要“顺手改进”相邻的代码、注释或格式
- 不要重构没有坏掉的东西
- 保持现有风格，即使你自己会写得不同
- 如果看到无关的死代码，提出来，不要擅自删除

当你的改动产生孤儿代码时：

- 删除因你的改动而变成未使用的导入、变量或函数
- 不要删除原本就存在的死代码，除非有人要求

**检验标准：** 每一行改动都应该能直接追溯到用户请求。

### 4. 目标驱动执行

**定义成功标准，循环验证直到通过。**

把命令式任务转成可验证的目标：

| 不要这样说 | 改成这样 |
|--------------|-----------|
| “加验证” | “为无效输入写测试，然后让它通过” |
| “修 bug” | “写一个能复现问题的测试，然后让它通过” |
| “重构 X” | “确保重构前后测试都能通过” |

对于多步骤任务，先写一个简短计划：

```
1. [步骤] → 验证: [检查]
2. [步骤] → 验证: [检查]
3. [步骤] → 验证: [检查]
```

清晰的成功标准能让代理自己循环推进；模糊的目标（“让它能用”）只会带来反复澄清。

## 安装

**选项 A：可复用技能（推荐）**

从 [`skills/karpathy-guidelines/SKILL.md`](skills/karpathy-guidelines/SKILL.md) 开始，把 [`skills/karpathy-guidelines/`](skills/karpathy-guidelines/) 目录复制或链接到你本地支持技能的目录中。其他代理如果也支持同样格式，就用它们自己的技能目录。

**选项 B：CLAUDE.md 兼容方式**

如果你的工具更适合项目级指令文件，可以把生成的 [`CLAUDE.md`](CLAUDE.md) 放到项目根目录，或者把它的内容合并进你现有的说明里。

## 在 Cursor 中使用

本仓库包含一个已提交的 Cursor 项目规则 ([`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc))，因此在 Cursor 中打开项目时同样会应用这些指南。详情请见 **[CURSOR.md](CURSOR.md)**，包括如何在其他项目中使用这份规则，以及它和 Claude Code 的关系。

## 生成产物

可编辑的单一事实来源是 [`skills/karpathy-guidelines/content.md`](skills/karpathy-guidelines/content.md)。

运行下面的命令可以重新生成派生文件：

```bash
python3 scripts/render_guideline_artifacts.py
```

这会更新：

- [`skills/karpathy-guidelines/SKILL.md`](skills/karpathy-guidelines/SKILL.md)
- [`CLAUDE.md`](CLAUDE.md)
- [`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc)

如果要检查这些生成文件是否仍然同步：

```bash
python3 scripts/render_guideline_artifacts.py --check
```

## 核心洞察

来自 Andrej：

> "LLM 非常擅长循环执行直到达成特定目标……不要告诉它该做什么，给它成功标准，然后看着它完成。"

“目标驱动执行”正是把这一点具体化：把命令式要求改写成带验证循环的声明式目标。

## 如何判断它在起作用

如果你看到这些迹象，说明指南在发挥作用：

- **diff 里少了不必要的改动** - 只出现用户要求的修改
- **因为过度复杂而返工更少** - 代码第一次就更简洁
- **澄清问题更早出现** - 而不是先犯错再问
- **PR 更干净** - 没有顺手重构或“优化”

## 定制

这套指南适合和项目特定指令一起使用。你可以把它们合并进现有的 `CLAUDE.md`，或者为项目单独准备一份。

对于项目特定规则，可以添加类似下面的章节：

```markdown
## 项目特定指南

- 使用 TypeScript 严格模式
- 所有 API 端点都必须有测试
- 遵循 `src/utils/errors.ts` 里的现有错误处理模式
```

## 权衡说明

这套指南偏向**谨慎而不是速度**。对于很小的任务，比如简单拼写修复或明显的一行修改，按场景判断即可，并不是每次都要走完整流程。

目标是减少非琐碎工作中的高代价错误，而不是拖慢简单任务。

## 贡献者说明

如果你要修改这四条原则，请编辑 [`skills/karpathy-guidelines/content.md`](skills/karpathy-guidelines/content.md)，然后重新生成派生文件：

```bash
python3 scripts/render_guideline_artifacts.py
python3 scripts/render_guideline_artifacts.py --check
```

不要手动编辑 [`CLAUDE.md`](CLAUDE.md)、[`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc) 或 [`skills/karpathy-guidelines/SKILL.md`](skills/karpathy-guidelines/SKILL.md)。

## 许可

MIT
