# X-SPRING-BOOT

## 简介 | Introduction

**X-SPRING-BOOT** 是一个基于 **Spring Boot 2.7** 的企业级后端脚手架，专为 SaaS 平台设计。它经过重构与优化，移除了冗余代码，专注于提供高性能、高扩展性的微服务就绪架构。

### 核心特性

- **多租户架构**：原生支持 SaaS 级多租户数据隔离，开箱即用。
- **低代码开发**：内置强大的代码生成器 (Code Generator)，支持 CRUD、表单、API 一键生成。
- **工作流引擎**：集成 Flowable 工作流，支持在线设计与审批流配置。
- **安全体系**：基于 Spring Security + JWT 实现完善的 RBAC 权限模型。
- **微服务就绪**：模块化设计，可随时从单体架构升级为 Spring Cloud 微服务架构。
- **基础设施**：集成 MySQL, Redis, MyBatis Plus, Redisson, Quartz 等主流技术栈。

## 目录结构 | Directory Structure

```text
x-spring-boot/
├── xera-dependencies/    # 统一依赖管理 (BOM)
├── xera-framework/       # 核心框架组件 (Web, Security, MyBatis, etc.)
├── xera-server/          # [启动模块] 后端主服务入口
├── xera-module-system/   # 系统模块 (用户, 角色, 菜单, 租户...)
├── xera-module-infra/    # 基础设施模块 (文件, 定时任务, 代码生成...)
├── sql/                  # 数据库初始化脚本
└── pom.xml               # 项目根 POM
```

## 快速开始 | Quick Start

### 1. 环境准备

- **JDK**: 1.8 / 11 / 17 / 21
- **Maven**: 3.6+
- **MySQL**: 8.0+
- **Redis**: 5.0+

### 2. 数据库初始化

1. 创建数据库 `ruoyi-vue-pro`。
2. 依次执行 `sql/mysql/ruoyi-vue-pro.sql` 和 `sql/mysql/quartz.sql`（如有）。

### 3. 配置文件

核心配置文件位于 `xera-server/src/main/resources/application-local.yaml` (本地开发) 或 `application.yaml` (通用)。

**修改数据库连接：**

```yaml
spring:
  datasource:
    dynamic:
      datasource:
        master:
          url: jdbc:mysql://127.0.0.1:3306/ruoyi-vue-pro...
          username: root
          password: your_password
```

**修改 Redis 连接：**

```yaml
spring:
  redis:
    host: 127.0.0.1
    port: 6379
```

### 4. 编译与启动

```bash
# 1. 编译打包 (跳过测试)
./mvnw clean package -Dmaven.test.skip=true

# 2. 启动服务
java -jar xera-server/target/xera-server.jar
```

启动成功后，接口文档访问地址：`http://localhost:48080/doc.html` (Knife4j)

## 容器化部署 | Docker Deployment

本项目支持全栈 Docker Compose 部署。

**1. 准备环境文件：**
- `xera-server.jar`
- `docker-compose.yml`
- `nginx.conf`
- 前端 `dist` 目录

**2. 启动服务：**

```bash
cd /opt/xera
docker compose up -d
```

**3. 查看日志：**

```bash
docker logs -f --tail 200 xera-server
```