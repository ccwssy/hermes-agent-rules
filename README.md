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

Hermes Agent 启动时自动加载此 skill，或手动加载：

```python
skill_view('agent-collaboration-rules')
```

## 依赖

- Hermes Agent v0.14.0+
- 无外部依赖

## 来源

- [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) — Karpathy 4 条准则（147k star）
- [shanraisshan/claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice) — Claude Code 最佳实践（54k star）
