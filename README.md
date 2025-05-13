# 京东多智能体智能导购助手项目

## 项目简介

本项目致力于开发一款基于多智能体技术的智能导购浏览器插件，支持京东、淘宝、小红书、抖音等多平台数据源，具备多轮对话、产品问答、横向比较、定制化推荐等功能，帮助用户高效选品、横向对比、智能决策，提升购物体验与满意度。

## 核心功能

1. **MCP接口调用**：聚合多平台商品、价格、评价等信息
2. **多轮对话**：支持自然语言多轮交互，理解用户意图
3. **产品问答**：智能解析用户问题，精准返回商品信息
4. **横向比较**：多商品多维度参数对比，辅助决策
5. **定制化推荐**：基于用户偏好和上下文智能推荐商品
6. **多平台数据源**：支持京东、淘宝、小红书、抖音等主流平台
7. **插件化架构**：浏览器插件形式，安装便捷，界面友好

## 目录结构说明

- `01_需求与规划/` —— 需求分析、技术选型、项目计划等文档
- `02_设计/` —— 系统架构、API、数据库、UI/UX等设计文档
- `03_开发与实现/` —— 前后端开发规范、代码结构、Agent设计等
- `04_测试/` —— 测试计划、用例、报告模板等
- `05_部署与上线/` —— 部署手册、上线检查清单等
- `06_运营与迭代/` —— 版本迭代计划、用户反馈收集等
- `src/` —— 项目源代码（开发中）

## 快速开始

### 1. 克隆项目
```bash
git clone https://github.com/LiuShiyanga/smart-shopping-guide
cd smart-shopping-guide
```

### 2. 安装依赖
```bash
# 前端插件
cd packages/extension
npm install

# 后端服务（如需）
cd ../server
pip install -r requirements.txt
```

### 3. 启动开发环境
```bash
# 前端插件热重载
npm run dev

# 后端服务（如需）
uvicorn main:app --reload
```

### 4. 构建与部署
详见 `05_部署与上线/部署手册.md`

## 主要技术栈
- 前端：React（Plasmo）、Ant Design、Zustand、TypeScript
- 后端：Python、FastAPI、SQLAlchemy、MongoDB/PostgreSQL、Redis
- 测试：Jest、Cypress、pytest
- 部署：Docker、Nginx、PM2、云服务器

## 开发规范与贡献指南
- 遵循各模块开发规范（见 `03_开发与实现/`）
- Git分支管理：main/develop/feature/bugfix/release
- 提交信息语义化（feat/fix/docs等）
- 代码需通过Lint和单元测试
- 所有合并需通过Pull Request评审

## 常见问题FAQ
- 插件无法加载/热重载失败：请检查依赖安装和端口占用
- 后端API无法访问：请检查服务是否启动、端口和防火墙配置
- 多平台数据获取异常：请确认API Key和平台接口权限
- 详细问题可查阅 `04_测试/` 和 `05_部署与上线/` 文档

## 团队与联系方式
- 技术支持邮箱：support@example.com
- 主要成员：产品经理、前端开发、后端开发、测试、运营等

## 版本/许可证
- 当前版本：v1.0.0（详见 `06_运营与迭代/版本迭代计划.md`）
- 许可证：仅限团队内部使用/开源协议（如MIT，视实际情况填写）

## 参考文档
- 需求规格说明书、系统架构设计、API接口设计、数据库设计、UI/UX设计、测试计划、部署手册、用户反馈模板等详见各阶段目录
- 相关链接：
  - [Plasmo官方文档](https://docs.plasmo.com/)
  - [FastAPI官方文档](https://fastapi.tiangolo.com/)
  - [Ant Design](https://ant.design/)
  - [京东开放平台](https://union.jd.com/helpcenter/13246-13247-46301) 