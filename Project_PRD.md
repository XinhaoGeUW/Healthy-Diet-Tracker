# 🥗 Healthy Diet Tracker — MVP PRD
## 📘 概述
**产品目标**  
Healthy Diet Tracker 是一款基于 Web 的健康饮食追踪应用，旨在帮助用户：
- 轻松记录每日饮食；
- 了解营养摄入与健康状况；
- 获得智能的个性化饮食建议；
- 建立长期健康饮食习惯。

**开发周期：** 约 4 周  
**目标平台：** Web（桌面 + 移动端自适应）  
**主要用户：** 有健康管理意识的普通人群（18–45 岁）

---

## 🧩 功能概览

| 优先级 | 模块名称 | 说明 |
|:--|:--|:--|
| P0 | 账号与基础画像 | 登录、画像问卷、营养目标计算 |
| P0 | 食物记录（手动+搜索） | 搜索食物、添加份量、自定义记录 |
| P0 | 今日概览仪表盘 | 显示热量和营养进度条 + 智能提示 |
| P0 | 餐次结构 | 早餐/午餐/晚餐/加餐的时间线记录 |
| P0 | 周报 | 一周趋势与总结 |
| P0 | 数据与隐私 | 数据导出与隐私声明 |
| P1 | 拍照识别（AI） | 餐盘识别并生成候选食物 |
| P1 | 个性化建议（AI） | 基于规则与 LLM 的个性化饮食建议 |
| P1 | 食谱搭配推荐 | 根据食材或目标搜索快速菜谱 |

---

## ⚙️ 技术架构与环境

### 前端
- 框架：React + TailwindCSS + Chart.js
- 构建工具：Vite / Next.js
- API 调用：Axios + React Query

### 后端
- 框架：Express.js (TypeScript)
- ORM：Prisma
- 测试：Jest + Supertest
- 环境管理：dotenv + cross-env
- API 架构：RESTful (可用 Swagger 生成接口文档)

### 数据库与文件存储
- PostgreSQL (Docker 容器内运行)
- MinIO (S3 兼容) 用于图片存储
- 本地使用 docker-compose 一键启动：
  ```bash
  docker-compose up -d
  ```

---

## 🧠 数据模型（简化）

```sql
users(id, email, password_hash, profile_json)
foods(id, name, nutrients_json)
meals(id, user_id, date, type)
meal_items(id, meal_id, food_id, quantity, unit, nutrients_snapshot)
photos(id, user_id, meal_id, image_url, ai_candidates_json, user_choice)
weekly_reports(id, user_id, week_start, summary_json)
```

---

## 📦 API 路由概要 (Express)

| 路由 | 方法 | 描述 |
|:--|:--|:--|
| /auth/register | POST | 注册新用户 |
| /auth/login | POST | 用户登录 |
| /profile | GET/PUT | 获取或更新个人画像 |
| /meals | GET/POST | 获取或创建每日餐次 |
| /meals/:id/items | POST | 添加食物项 |
| /foods | GET | 搜索食物 |
| /upload | POST | 上传餐盘图片到 MinIO |
| /ai/recognize | POST | 调用食物识别模型 |
| /ai/suggestions | GET | 返回个性化建议 |
| /reports/weekly | GET | 获取周报数据 |
| /export/csv | GET | 导出饮食数据 CSV |

---

## 🧰 Docker 环境说明

**docker-compose.yml**（含 Postgres + MinIO + pgAdmin）
```bash
docker-compose up -d
```
服务：
- Postgres: `localhost:5432` (用户: app / 密码: app123)
- MinIO: `localhost:9000` (访问密钥: minio / minio12345)
- pgAdmin: `localhost:5050`

**.env 环境变量**
```
POSTGRES_USER=app
POSTGRES_PASSWORD=app123
POSTGRES_DB=healthy
MINIO_ROOT_USER=minio
MINIO_ROOT_PASSWORD=minio12345
PGADMIN_EMAIL=admin@example.com
PGADMIN_PASSWORD=admin123
```

---

## 🚀 里程碑计划（4 周）

| 周 | 目标 |
|--|--|
| Week 1 | 账号系统 + 画像 + 手动食物记录 + 仪表盘基础 |
| Week 2 | 周报 + 数据导出 + 餐次结构完善 |
| Week 3 | 拍照识别 + 个性化建议 MVP |
| Week 4 | 食谱推荐 + UI 打磨 + 测试部署 |

---

## ✅ Demo 检查清单
- [ ] 登录注册流程顺畅可用  
- [ ] 能搜索并添加食物  
- [ ] 仪表盘正确显示进度条  
- [ ] 导出 CSV 文件内容完整  
- [ ] 上传餐盘图片能触发识别流程  
- [ ] 显示个性化饮食建议  
- [ ] 周报数据可视化正常  
