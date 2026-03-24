---
title: "MMORPG开发日志01：背包系统的设计与实现"
description: "记录背包系统从零到完成的全过程"
date: 2026-03-24T14:00:00+08:00
image: https://images.unsplash.com/photo-1614853316476-de00d14cb1fc?w=1200
tags:
  - Unity
  - C#
  - MMORPG
categories:
  - 开发日志
draft: false
---

## 今天做了什么？

完成了MMORPG项目的背包系统，支持物品添加、删除、堆叠和排序。

## 技术设计

使用List存储物品列表，ScriptableObject定义物品模板。

## 遇到的问题

### 问题1：物品堆叠溢出

**现象：** 背包满时继续拾取可堆叠物品，数量会超过上限。

**解决：** 在AddItem方法中添加堆叠上限检查。

### 问题2：UI刷新不及时

**现象：** 拖拽物品后UI显示不同步。

**解决：** 使用观察者模式，数据变更时触发UI刷新事件。

## AI的帮助

用Cursor IDE加速了代码编写，用Ollama帮我Review了核心逻辑。

## 下一步计划

- 实现装备系统
- 添加物品排序功能
- 优化UI拖拽手感