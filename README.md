# Git 提交规则

本文档定义本仓库推荐的分支、提交、暂存、验证、推送和 Pull Request 规则。

## 基本原则

- 只有在用户或维护者明确要求时才创建提交；不要在完成修改后自动提交。
- 每个提交只表达一个逻辑上独立、可测试的变更。
- 不要把无关的功能、修复、重构、测试或文档改动混在同一个提交中。
- 当一组变更包含多个类型或多个功能范围时，拆成多次提交。
- 不要使用 `--no-verify` 绕过检查。
- 不要重写其他协作者已经基于其工作的共享历史。

## 分支命名

推荐使用以下格式：

- `codex/<topic>`
- `feature/<topic>`
- `fix/<topic>`
- `docs/<topic>`

`<topic>` 使用简短、清晰的英文 kebab-case，例如 `codex/git-rules`、`fix/login-timeout`。

## 提交信息格式

使用 Conventional Commits：

```text
<type>(<scope>): <subject>
```

如果是仓库级别变更，可以省略 `scope`：

```text
<type>: <subject>
```

### type

推荐类型：

- `feat`: 新功能或新增能力
- `fix`: 缺陷修复
- `refactor`: 不改变外部行为的重构
- `test`: 测试新增或修改
- `docs`: 文档变更
- `chore`: 构建、配置、依赖、维护类变更
- `perf`: 性能优化

一个提交只使用一种 `type`。如果同一批改动同时包含功能、文档和重构，应拆成多个提交。

### scope

`scope` 表示受影响的区域，例如：

- 算法或功能名：`grpo`、`opd`
- 共享库：`rl_common`
- 子模块：`rl_common/trainer`
- 产品或流程模块：`generation`、`runner`、`verification`

如果变更影响整个仓库，可以省略 `scope`。

### subject

`subject` 应满足：

- 使用英文
- 使用祈使语气
- 小写开头
- 不以句号结尾
- 尽量不超过 72 个字符

示例：

```text
feat(grpo): add group-relative advantage loss
fix(runner): preserve tool errors in trajectory
refactor(rl_common): extract shared base trainer
test(verification): cover hard-check reward gating
docs(architecture): explain environment boundary
docs: add git commit rules
```

## 提交正文

对行为变化、数据格式变化或兼容性风险较高的提交，建议在提交正文说明：

- 变更动机
- 主要实现思路
- 数据、Schema 或导出格式影响
- 兼容性影响
- 验证方式和测试结果
- 可能的回滚方式

## 暂存规则

- 使用显式路径暂存文件，例如 `git add README.md`。
- 不要使用 `git add -A` 或 `git add .`。
- 提交前先查看 `git status --short` 和 `git diff --cached`。
- 不要暂存本地实验脚本、临时输出、模型文件、数据集、密钥或缓存文件。

禁止提交：

- `.env`、API keys、tokens、凭证文件
- `runs/`、`outputs/`、`checkpoints/`
- `models/`、`datasets/`、模型权重、未授权数据
- `__pycache__/`、`.pytest_cache/`、构建缓存
- 大型生成产物或一次性 smoke-test 输出

## 验证规则

提交前应完成与变更范围匹配的验证：

- 文档变更：检查格式、链接和示例命令是否正确。
- 代码变更：确保导入可解析，相关单元测试或离线 smoke test 通过。
- 行为变更：补充或更新测试，并记录关键验证结果。
- 数据或 Schema 变更：说明迁移、兼容性和回滚方案。

如果无法运行某项检查，应在提交说明、PR 描述或交付说明中明确说明原因。

## 推送规则

- 完成用户或维护者要求的本地提交后，推送到配置好的远端。
- 如果用户明确要求只保留本地提交，或仓库没有可用远端，则不要推送。
- 推送前确认当前分支和远端目标正确。

## Pull Request 规则

PR 描述应覆盖：

- 问题背景
- 解决方案
- 数据、Schema 或兼容性影响
- 测试结果
- 成本、安全或隐私影响
- 回滚计划

PR 应保持聚焦。不要把多个不相关主题放进同一个 PR。

## 推荐工作流

```bash
git status --short
git diff
git add <explicit-path>
git diff --cached
git commit -m "docs: add git commit rules"
git push origin <branch>
```
