# 🧠 Hermes Memory Management Guide

> 让 [Hermes Agent](https://github.com/NousResearch/hermes-agent) 的记忆从臃肿混乱到清爽高效——经过实战验证的方法论。

<p align="center">
  <strong>📦 30+ 条目 → 2 条目 &nbsp;|&nbsp; 📉 60% 占用 → 4% 占用 &nbsp;|&nbsp; ⚡ 一次清理，长期清爽</strong>
</p>

---

## 🤔 你遇到过吗？

- 记忆越来越臃肿，system prompt 里塞了 4000 字的陈年旧账
- 每次对话注入大量过时信息，Agent 反应越来越慢
- 重要配置和半年前的聊天记录混在一起，找不到想要的东西
- 多 Profile 跑在同一台服务器上，文件隔离全靠「信任」

**这套方法论就是为此设计的。**

---

## 💡 核心哲学

```
❌ 把所有东西塞进记忆 → 快速占满 → 被迫删旧存新 → 丢失有价值信息
✅ 文档优先，记忆做索引 → 内存轻量 → 按需读取 → 永不过期
```

### 三层信息架构

```
📁 独立文档（~/docs/）
   ├── 详细笔记、项目记录、偏好文档
   └── 按需 read_file，不占记忆空间
        ↕
📋 记忆索引（Memory 中 1 条）
   └── 指向索引文件 → 索引文件指向所有文档
        ↕
⭐ 关键速查（Memory 中 1-2 条）
   └── 高频参数摘要，<200 字
```

---

## 📂 仓库内容

| 文件 | 内容 | 阅读顺序 |
|------|------|---------|
| **[hermes-memory-management.md](./hermes-memory-management.md)** | 日常管理理念：三层架构、决策树、新增信息规则、多 Profile 隔离 | 🥇 先读 |
| **[hermes-memory-cleanup.md](./hermes-memory-cleanup.md)** | 批量清理流程：审计→归类→建文档→删冗余→验证（六步） | 🥈 记忆膨胀时读 |

---

## 🚀 快速开始

### 1. 建立索引文件

在 Profile 私有目录创建索引：

```bash
mkdir -p ~/.hermes/profiles/default/private/
```

```markdown
# Memory Index
| # | 文档 | 路径 | 内容 |
|---|------|------|------|
| 1 | 系统运维 | `~/system-ops.md` | 服务器配置、SSH、安全策略 |
| 2 | 项目笔记 | `~/project-notes.md` | 技术栈、架构、里程碑 |
```

### 2. 更新 Hermes 记忆

Memory 中只留一条索引指针：

```
所有记忆档案见 ~/.hermes/profiles/default/private/memory-index.md，需要时 read_file 读取。
```

### 3. 日常新增信息

按决策树判断放哪里：

```
收到重要信息
  ├─ 行为偏好 / 沟通风格？ → User Profile
  ├─ 详细但不必每次看？ → 写文档 → 更新索引
  └─ 每次都要看的参数？ → Memory（精简！）
```

### 4. 定期清理

记忆占用超 50% 时，执行[六步清理流程](./hermes-memory-cleanup.md)。

---

## 📊 实测效果

| 指标 | 清理前 | 清理后 |
|------|--------|--------|
| 条目数 | 30+ | 2 |
| 占用率 | 60% (3961/6600 chars) | 4% (303/6600 chars) |
| 分类 | 散落无序 | 按主题归档 17 份文档 |
| 可维护性 | 每次对话灌 4KB 冗余 | 索引 + 速查，按需读取 |

---

## 🔒 多 Profile 安全隔离

同一服务器跑多个 Hermes Profile 时，敏感文件不要放 `~/`：

```bash
# ❌ 其他 Profile 的 Agent 也能读到
~/sensitive-notes.md

# ✅ 仅当前 Profile 私有目录
~/.hermes/profiles/<name>/private/sensitive-notes.md
```

详见 [hermes-memory-management.md](./hermes-memory-management.md) 的「Profile 隔离」章节。

---

## 📄 许可

MIT © 2026

---

## ⭐ 如果对你有用

欢迎 Star & 分享给用 Hermes 的朋友。有问题提 Issue，有更好的实践提 PR 🙌

---
*Last updated: 2026-06-27*