# API接口设计文档

## 1. 接口设计原则
- 前后端分离，全部采用RESTful风格，数据格式为JSON
- 统一入口由中央协调器Orchestrator负责分发
- 各Agent以独立服务或模块形式暴露接口，便于扩展
- 支持多平台数据源（京东、淘宝、小红书、抖音等）
- 接口命名规范、参数清晰、响应结构统一
- 兼容性与可扩展性优先，支持版本管理

## 2. 接口总览

| 接口名称 | 方法 | 路径 | 说明 |
|----------|------|------|------|
| 多轮对话 | POST | /api/dialog | 用户与导购助手的主对话入口 |
| 商品搜索 | POST | /api/search | 多平台商品关键词检索 |
| 商品比较 | POST | /api/compare | 多商品横向对比 |
| 产品推荐 | POST | /api/recommend | 个性化/场景化推荐 |
| 产品问答 | POST | /api/qa | 产品相关问答 |
| 多平台数据获取 | POST | /api/fetch | 指定平台商品/评价等原始数据获取 |

## 3. 详细接口定义

### 3.1 多轮对话接口
- 路径：`/api/dialog`
- 方法：POST
- 请求参数：
```json
{
  "session_id": "string", // 会话唯一标识
  "user_input": "string", // 用户输入
  "context": { ... } // 可选，对话上下文
}
```
- 响应参数：
```json
{
  "reply": "string", // 智能助手回复
  "actions": [ ... ], // 可选，推荐、比较等动作
  "context": { ... } // 新的对话上下文
}
```
- 错误码：
  - 40001：参数缺失或格式错误
  - 50001：内部服务异常

### 3.2 商品搜索接口
- 路径：`/api/search`
- 方法：POST
- 请求参数：
```json
{
  "keyword": "手机",
  "platforms": ["jd", "taobao", "xiaohongshu", "douyin"],
  "filters": { "price_min": 1000, "price_max": 3000 },
  "sort_by": "price|sales|rating"
}
```
- 响应参数：
```json
{
  "results": [
    { "platform": "jd", "product_id": "123", "name": "...", "price": 1999, "url": "...", ... },
    { "platform": "taobao", ... }
  ]
}
```
- 错误码：
  - 40001：参数缺失或格式错误
  - 40002：不支持的平台
  - 50002：平台接口异常

### 3.3 商品比较接口
- 路径：`/api/compare`
- 方法：POST
- 请求参数：
```json
{
  "products": [
    { "platform": "jd", "product_id": "123" },
    { "platform": "taobao", "product_id": "abc" }
  ],
  "fields": ["price", "brand", "specs", "rating"]
}
```
- 响应参数：
```json
{
  "comparison": [
    { "field": "price", "jd": 1999, "taobao": 1988 },
    { "field": "brand", "jd": "小米", "taobao": "小米" },
    ...
  ]
}
```
- 错误码：
  - 40001：参数缺失或格式错误
  - 40003：商品信息获取失败
  - 50003：比较服务异常

### 3.4 产品推荐接口
- 路径：`/api/recommend`
- 方法：POST
- 请求参数：
```json
{
  "scene": "送礼|自用|老人|运动",
  "user_input": "我想买一款适合跑步的蓝牙耳机",
  "platforms": ["jd", "taobao"],
  "history": [ ... ] // 可选，用户历史
}
```
- 响应参数：
```json
{
  "recommendations": [
    { "platform": "jd", "product_id": "...", "name": "...", "reason": "轻便防水，适合运动" },
    ...
  ]
}
```
- 错误码：
  - 40001：参数缺失或格式错误
  - 50004：推荐服务异常

### 3.5 产品问答接口
- 路径：`/api/qa`
- 方法：POST
- 请求参数：
```json
{
  "product": { "platform": "jd", "product_id": "123" },
  "question": "这款手机防水吗？"
}
```
- 响应参数：
```json
{
  "answer": "支持IP68级防水。",
  "source": "jd"
}
```
- 错误码：
  - 40001：参数缺失或格式错误
  - 40004：答案未找到
  - 50005：问答服务异常

### 3.6 多平台数据获取接口
- 路径：`/api/fetch`
- 方法：POST
- 请求参数：
```json
{
  "platform": "jd|taobao|xiaohongshu|douyin",
  "type": "product|review|promotion",
  "id": "商品或内容ID"
}
```
- 响应参数：
```json
{
  "data": { ... } // 平台原始数据结构
}
```
- 错误码：
  - 40001：参数缺失或格式错误
  - 40005：平台数据获取失败
  - 50006：数据服务异常

## 4. 认证与安全
- 所有接口需通过API Key或Token认证（如有后端部署）
- 敏感信息不在前端暴露，所有平台API密钥仅后端持有
- 前后端通信采用HTTPS
- 支持接口访问日志与异常监控

## 5. 限流与性能
- 单用户QPS限制（如每秒5次）
- 全局接口限流，防止恶意刷接口
- 支持接口超时与重试机制

## 6. 版本管理
- 所有接口支持URL版本号（如/v1/api/...）
- 版本升级兼容旧接口，废弃接口提前公告

## 7. 错误处理
- 统一错误码与错误信息结构：
```json
{
  "code": 40001,
  "message": "参数缺失或格式错误",
  "data": null
}
```
- 所有错误均有详细日志，便于排查

## 8. 接口变更策略
- 重大变更提前公告，提供迁移文档
- 兼容性变更支持灰度发布与回滚

## 9. 参考文档
- 《系统架构设计文档》
- 《数据库设计说明书》
- 各平台API/开放平台文档
- RESTful API设计最佳实践

---
如需OpenAPI/Swagger文档可后续自动生成。 