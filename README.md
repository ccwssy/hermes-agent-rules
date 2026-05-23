# Hermes Agent 协作规则

从 [Andrej Karpathy 4 条 LLM 行为准则](https://github.com/multica-ai/andrej-karpathy-skills) + [Claude Code 最佳实践](https://github.com/shanraisshan/claude-code-best-practice) 提炼，适配 Hermes Agent 的通用协作规则。

## 规则内容

### Karpathy 4 条核心行为准则
1. **先想再写** — 明确假设，呈现权衡，不确定就问
2. **简单至上** — 最少代码解决问题，不写推测性东西
3. **精确改动** — 只碰必须碰的，只清理自己的垃圾
4. **目标驱动** — 定义验证标准，循环到通过

### Hermes 适配扩展
5. **Subagent 设计** — delegate_task 渐进披露原则
6. **上下文管理** — context window 预算控制
7. **权限安全** — 报告→等待确认流程
8. **输出风格** — 用户偏好适配
9. **经验沉淀** — 持续改进循环

完整内容见 [SKILL.md](SKILL.md)。

## 安装

### 方式一：Hermes tap（推荐）

```bash
hermes skills tap add ccwssy/hermes-agent-rules
hermes skills install agent-collaboration-rules
```

### 方式二：直接下载

```bash
# 下载
curl -L https://github.com/ccwssy/hermes-agent-rules/archive/refs/heads/main.tar.gz | tar xz

# 放到技能目录
mv hermes-agent-rules-main ~/.hermes/skills/agent-collaboration-rules
```

### 方式三：Git clone

```bash
git clone git@github.com:ccwssy/hermes-agent-rules.git ~/.hermes/skills/agent-collaboration-rules
```

## 使用

### ⚠️ 重要：安装 ≠ 生效

装完 skill 只是把文件放进了目录。**Hermes 默认不会自动加载已安装的 skill**，需以下任一方式激活：

### 激活方式

| 方式 | 命令 | 范围 | 持久性 |
|------|------|------|--------|
| **手动加载** | `/skill agent-collaboration-rules`<br>`skill_view('agent-collaboration-rules')` | 当前会话 | ❌ 仅当前会话 |
| **CLI 预加载** | `hermes -s agent-collaboration-rules` | 单次启动 | ❌ 仅该次 CLI |
| **全局启用** | `hermes skills config --enable agent-collaboration-rules` | 所有新会话 | ✅ 永久生效 |

### 进阶：SOUL.md 自动调度（推荐）

在 `~/.hermes/SOUL.md` 加入以下配置，让 Agent **在会话开始时一次性扫描所有 skill**，之后按需加载，不重复扫描：

```markdown
## Skill 自动调度
- 会话开始时：调用 skills_list() 扫描一次所有可用 skill，建立索引
- 每轮回复前：不重新全量扫描，根据当前任务关键词判断是否需要加载已知 skill
- 加载后该 skill 规则在当前会话持续生效
- 会话中途安装了新 skill，需 /reload-skills 刷新索引
```

**优化点对比：**

| 维度 | 旧方案（每轮扫描） | 新方案（会话开始一次） |
|------|-------------------|----------------------|
| 每轮对话扫描 | 全部 skill | 0 次（仅索引匹配） |
| 10 轮 100 skill | 1000 次操作 | 100 次操作固定 |
| 100+ skill 压力 | 线性增长 | 固定成本 |
| 中途装新 skill | 下轮自动发现 | ❌ 需 /reload-skills |

> 旧方案在 skill 少时无感，但 50+ 后每轮扫描的开销会开始明显。新方案牺牲了"中途安装自动发现"（低频需求）换取稳定的性能。

## 依赖

- Hermes Agent v0.14.0+
- 无外部依赖

## 来源

- [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) — Karpathy 4 条准则（147k star）
- [shanraisshan/claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice) — Claude Code 最佳实践（54k star）
