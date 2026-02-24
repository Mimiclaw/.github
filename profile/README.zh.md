# Mimiclaw

> 构建你的 AI 员工体系
> 编排自治智能体
> 让 OpenClaw 成为一家公司

<img width="1536" height="459" alt="banner-sm" src="https://github.com/user-attachments/assets/ce7441ba-559f-441f-837b-896e08926b96" />

[English](readme.md) | **中文**
---

## Mimiclaw 是什么？

Mimiclaw 是 OpenClaw 的 **“人力层（Workforce Layer）” Skill 模块**。

它不是另一个 Agent。
不是多 Agent Demo。
也不是简单的任务分发器。

Mimiclaw 的作用是：

让 OpenClaw 能够：

* 创建独立员工 Agent
* 分配职责
* 并行执行任务
* 监控阶段性进度
* 对资产行为进行审批控制
* 全生命周期管理与销毁
* 通过可视化看板观察整个组织结构

它将“智能”转化为“组织”。

---

## 核心理念

任何单一智能体都会遇到同一个问题：

> 单个 Agent 无法像公司一样扩展。

Mimiclaw 解决的不是“智能能力不足”。

而是“组织能力不足”。

通过 Mimiclaw，OpenClaw 可以：

* 生成多个独立员工
* 为其设定明确分工
* 允许员工之间协作
* 并行执行复杂任务
* 阶段性汇报进度
* 在涉及资产操作时触发审批
* 随时销毁、替换、重组

这不是 Agent 链式调用。

这是结构化的数字劳动力系统。

---

## 概念结构

OpenClaw 作为监督者（Supervisor）。
员工作为执行单元（Workers）。

```text
                ┌────────────────────┐
                │      OpenClaw      │
                │   （监督核心）      │
                └─────────┬──────────┘
                          │
             ┌────────────┼────────────┐
             │            │            │
        ┌────▼────┐  ┌────▼────┐  ┌────▼────┐
        │ 员工 1    │  │ 员工 2    │  │ 员工 3    │
        │ (picoclaw)│  │ (picoclaw)│  │ (picoclaw)│
        └──────────┘  └──────────┘  └──────────┘
```

OpenClaw 负责方向。
Mimiclaw 负责执行能力扩展。

---

## 架构哲学

Mimiclaw 建立在五个核心原则之上：

---

### 1. 隔离（Isolation）

每个员工：

* 独立 picoclaw 进程
* 独立 workspace（由 picoclaw 约束）
* 拥有独立：

  * Skill
  * 工具权限
  * API Key（本地明文）
  * 钱包抽象
  * Memory
* 可随时销毁

员工不可递归生成员工。

控制权始终归属于 OpenClaw。

---

### 2. 并行（Parallelism）

* 员工并行执行任务
* 优先级仅用于排序（不抢占）
* 用户指定最大并行员工数量
* 达到上限时：

  * OpenClaw 决定销毁或取舍

这模拟公司内部的资源分配逻辑。

---

### 3. 结构化通信（Structured Communication）

所有通信均为本地 WebSocket。

* OpenClaw ↔ 员工
* 员工 ↔ 员工

消息结构统一为 JSON。

支持：

* 任务分配
* 阶段性进度上报
* 最终结果提交
* 心跳检测
* 资产审批请求

OpenClaw 记录所有通信。

这构成完整的组织通讯图谱。

---

### 4. 审批式资产安全（Approval-Based Asset Security）

员工可以发起资产操作请求。

但不能签名。

流程：

1. 员工发起资产操作申请
2. OpenClaw 请求用户审批
3. 用户确认
4. OpenClaw 代签
5. 结果返回员工继续执行

签名权永远不下放。

---

### 5. 监督控制（Supervisor Model）

OpenClaw 是：

* 任务分配者
* 审批执行者
* 生命周期管理者
* 健康监控者
* WebSocket 通信中心

员工只负责执行。

权力集中。

---

## 健康模型

员工健康由三重信号判定：

1. 进程存在
2. WebSocket 心跳正常
3. Agent 自汇报状态

任何异常均由 OpenClaw 处理。

---

## JSON 协议

### 任务结构

```json
{
  "task_id": "uuid",
  "from": "openclaw",
  "to": "employee-1",
  "priority": 5,
  "goal": "Analyze smart contract risk",
  "inputs": {},
  "tools_allowed": ["bash", "curl"],
  "assets_flag": false,
  "approval_required": false,
  "timestamp": 1700000000
}
```

---

### 进度上报结构

```json
{
  "task_id": "uuid",
  "status": "in_progress",
  "progress": 0.45,
  "message": "Parsing contract ABI...",
  "timestamp": 1700000100
}
```

---

### 资产审批请求

```json
{
  "task_id": "uuid",
  "type": "asset_request",
  "action": "transfer",
  "payload": {
    "to": "0x...",
    "amount": "1.5 ETH"
  }
}
```

---

### 结果结构

```json
{
  "task_id": "uuid",
  "status": "completed",
  "result": {},
  "timestamp": 1700000200
}
```

---

## Dashboard 愿景

Mimiclaw 提供组织可视化看板。

展示：

* 员工列表
* 当前任务
* 优先级排序
* 进度节点
* 通信日志
* 审批记录
* 架构拓扑图

示例：

```text
[OpenClaw]
    │
    ├── [Researcher-1]
    ├── [Coder-1]
    └── [Operator-1]
```

这是一个可观察的 AI 组织。

不是黑盒执行。

---

## 多模型策略

* 模型由 OpenClaw 分配
* 可按员工或按任务指定
* 员工无模型自主权

决策权始终集中。

---

## 生命周期接口

Mimiclaw 提供完整控制能力：

* create employee
* delete employee
* assign task
* broadcast task
* list employees
* get status
* fetch logs

整个公司是可编程的。

---

## 适用场景

* 自治 AI 公司
* DAO 自动运营
* 并行研究团队
* 自动化代码生产线
* 交易辅助系统
* 安全审计体系

---

## Mimiclaw 不是什么

* 不是代币系统
* 不是治理协议
* 不是资源配额系统
* 不是递归智能体系统
* 不是云成本优化平台

它是执行架构。

---

## 更大的愿景

Mimiclaw 所代表的是：

从：

单一智能体

到：

结构化 AI 员工体系

从：

工具调用

到：

组织化数字劳动

从：

AI 助手

到：

AI 公司

---

## 当前状态

设计完成
协议已定义
WebSocket 架构实现中
进程托管文档待补充

---

## License

MIT
