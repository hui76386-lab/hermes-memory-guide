---
name: hermes-memory-management
description: "Hermes 记忆管理最佳实践：文档优先，记忆做索引。适用于任何 Hermes profile。"
version: 1.0.0
---

# Hermes 记忆管理最佳实践

> **核心哲学**：文档优先（Documents First），记忆做索引（Memory as Index）

## 为什么需要这套方法

Hermes 的记忆系统有两个特点：

1. **容量有限**：Memory 和 User Profile 各有字符上限（默认 6600 / 4000 chars）
2. **每次注入**：记忆内容在每个对话轮次都会注入 system prompt，占用上下文窗口

把详细内容直接塞进记忆 → 快速占满 → 被迫删旧存新 → 丢失有价值信息。

**解决方案**：记忆只存索引指针，详细内容放独立文档。

## 三层信息架构

```
📁 独立文档（~/docs/）
  ├── 详细的技术笔记、项目记录、偏好文档
  ├── 按需 read_file 读取
  └── 不占记忆空间
📋 记忆索引（Memory 中 1 条）
  └── 指向所有文档的目录文件路径
⭐ 关键速查（Memory 中 1-2 条）
  └── 高频使用的参数摘要
👤 用户画像（User Profile）
  └── 行为偏好、沟通风格、底线规则
```

## 日常工作流

收到需要记住的信息时，按以下决策树行动：

```
收到重要信息
  ├─ 是用户的行为偏好 / 沟通风格 / 底线规则？
  │   └─ 存 User Profile（memory tool, target='user'）
  ├─ 是需要保留但不紧急的详细内容？
  │   └─ ① 创建或追加到对应文档
  │      ② 更新索引文件
  │      ③ 在 Memory 中更新索引指针
  └─ 是需要每次会话都看到的关键参数？
      └─ 直接存 Memory（精简！控制在 200 字内）
```

## 索引文件

在 profile 私有目录下维护一个索引文件（如 `private/memory-index.md`）：

```markdown
# Memory Index

## 分类一
| # | 文档 | 路径 | 内容 |
|---|------|------|------|
| 1 | 项目笔记 | `~/project-notes.md` | 项目技术栈、架构、里程碑 |
| 2 | 用户偏好 | `~/user-preferences.md` | 审美偏好、工作习惯 |

## 分类二
...
```

Memory 中只存一行：

```
所有记忆档案索引见 ~/.hermes/profiles/<name>/private/memory-index.md，需要时 read_file 读取。
```

## Profile 隔离与敏感文件保护

### 背景

同一服务器上运行多个 Hermes Profile 时，它们虽拥有独立的 skills/memories/cron，但共享同一文件系统（同一 Linux 用户身份）。

### 风险

Profile A 的 agent 技术上有能力 `read_file` 读取 Profile B 放在 `~/` 下的文件。

### 解决方案

敏感档案放入 profile 专属目录：

```bash
mkdir -p ~/.hermes/profiles/<name>/private/
mv ~/sensitive-file.md ~/.hermes/profiles/<name>/private/
```

移动后同步更新索引文件和 Memory 中的路径引用。

### 原则

- Profile 隔离是**逻辑层面**（人设、记忆、技能），不是文件系统沙箱
- 技术文档可留在 `~/`，私密内容放 `private/`
- 此原则适用于任何多 profile 部署

## 配合使用

本技能定义日常管理规则。当记忆膨胀需要大扫除时，使用 `hermes-memory-cleanup` 技能执行批量清理。

---

## 常见问题

**Q: 什么该放 Memory，什么该放文档？**

| 放 Memory | 放文档 |
|-----------|--------|
| 索引指针 | 详细技术笔记 |
| 高频参数速查（<200字） | 项目背景和架构 |
| — | 历史事件记录 |
| — | 可分类的偏好详情 |

**Q: 多久清理一次？**

记忆占用超过 50% 时执行清理。通常每 2-4 周一次，取决于使用频率。

**Q: 文档放在哪？**

- 通用文档：`~/`
- 敏感文档：`~/.hermes/profiles/<name>/private/`
- 技能相关：对应 skill 的 `references/` 目录
