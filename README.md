# Hermes Memory Management Guide

> 让 Hermes Agent 的记忆保持清爽的实战指南

## 这是什么

一套经过实战验证的 Hermes 记忆管理方法论。核心理念：**文档优先，记忆做索引**。

如果你遇到过这些问题：
- Hermes 记忆越来越臃肿，占用率 60%+
- 对话越来越慢，上下文被记忆撑满
- 重要信息和过时信息混在一起
- 多个 Profile 之间文件隔离不清楚

这个仓库就是为你准备的。

## 两份文档

| 文档 | 用途 | 什么时候读 |
|------|------|-----------|
| [hermes-memory-management.md](./hermes-memory-management.md) | 日常管理理念和规则 | 先读这个，建立体系 |
| [hermes-memory-cleanup.md](./hermes-memory-cleanup.md) | 批量清理六步工作流 | 记忆膨胀时执行 |

## 快速开始

1. 在 Hermes 中导入 `hermes-memory-management` 作为技能
2. 建立你的索引文件（参考文档中的模板）
3. 日常新增信息走三层架构：文档 ← 索引 ← 速查
4. 记忆超 50% 时，执行 `hermes-memory-cleanup` 六步清理

## 效果

实测：30+ 条目 → 2 条目，占用率 60% → 4%

## 许可

MIT
