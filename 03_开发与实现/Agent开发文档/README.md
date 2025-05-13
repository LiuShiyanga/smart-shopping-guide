# 多智能体开发文档

本目录存放京东多智能体智能导购助手的Agent开发相关文档。多智能体架构是本项目的核心特色，通过不同功能的智能体协同工作，实现智能导购助手的全部功能。

## 智能体概述

本项目采用多智能体架构，主要包含以下几类智能体：

1. **对话管理Agent** - 负责自然语言理解、对话流程管理和回复生成
2. **信息查询Agent** - 负责调用MCP接口获取商品信息、价格、评价等数据
3. **产品比较Agent** - 负责同类产品的多维度对比和分析
4. **推荐Agent** - 负责基于用户偏好和上下文进行产品推荐
5. **UI交互Agent** - 负责界面展示和用户交互管理

## 智能体架构设计

多智能体间采用消息传递机制进行通信，由中央协调器管理各智能体的协作。具体设计请参考文档：

- [`AgentFramework.md`] - 多智能体框架设计
- [`DialogAgent.md`] - 对话管理Agent设计
- [`InfoAgent.md`] - 信息查询Agent设计
- [`ComparisonAgent.md`] - 产品比较Agent设计
- [`RecommendationAgent.md`] - 推荐Agent设计
- [`UIAgent.md`] - UI交互Agent设计
- [`Coordinator.md`] - 智能体协调器设计

## 开发规范

### 智能体接口规范

每个Agent需要实现以下标准接口：

```typescript
interface Agent {
  // Agent初始化
  initialize(): Promise<void>;
  
  // 处理输入消息
  handleMessage(message: AgentMessage): Promise<AgentMessage>;
  
  // 获取Agent状态
  getState(): AgentState;
  
  // 重置Agent状态
  reset(): void;
}
```

### 消息格式规范

智能体间的消息采用标准JSON格式：

```typescript
interface AgentMessage {
  messageId: string;        // 消息唯一标识
  fromAgent: string;        // 发送Agent ID
  toAgent: string;          // 接收Agent ID
  messageType: string;      // 消息类型
  content: any;             // 消息内容
  timestamp: number;        // 时间戳
  conversationId: string;   // 会话ID
}
```

### 智能体状态管理

- 每个智能体需要维护自己的状态
- 状态变更需要通知协调器
- 协调器根据各智能体状态进行任务分配和协调

## 开发流程

1. 根据智能体接口规范实现各智能体基类
2. 实现具体业务逻辑
3. 实现智能体间通信机制
4. 实现智能体协调器
5. 单元测试各智能体功能
6. 集成测试多智能体协作

## 测试指南

- 每个智能体需要编写单元测试，覆盖核心功能
- 模拟多场景下的智能体协作，进行集成测试

## 性能优化建议

- 异步处理耗时操作
- 适当使用缓存减少重复计算和网络请求
- 监控智能体资源占用，防止单个智能体成为性能瓶颈 