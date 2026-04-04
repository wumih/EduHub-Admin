<div align="center">

# 🏢 Tlias Web Management System

<p align="center">
  基于 Spring Boot 3 + MyBatis + JWT 的企业级人员与教务综合管理平台
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Spring%20Boot-3.5.6-brightgreen?style=for-the-badge&logo=springboot" alt="Spring Boot"/>
  <img src="https://img.shields.io/badge/MyBatis-3.0.5-blue?style=for-the-badge&logo=databricks" alt="MyBatis"/>
  <img src="https://img.shields.io/badge/Java-21-orange?style=for-the-badge&logo=openjdk" alt="Java"/>
  <img src="https://img.shields.io/badge/MySQL-8.x-blue?style=for-the-badge&logo=mysql" alt="MySQL"/>
  <img src="https://img.shields.io/badge/Aliyun%20OSS-Custom%20Starter-ff6a00?style=for-the-badge&logo=alibabacloud" alt="Aliyun OSS"/>
  <img src="https://img.shields.io/badge/JWT-0.11.5-purple?style=for-the-badge&logo=jsonwebtokens" alt="JWT"/>
  <img src="https://img.shields.io/badge/Docker-Ready-2496ed?style=for-the-badge&logo=docker" alt="Docker"/>
  <img src="https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge" alt="MIT License"/>
</p>

</div>

---
🔗 **相关项目**：
- [前端项目仓库](https://github.com/wumih/EduHub-Admin-front.git)
- [后端项目仓库](https://github.com/wumih/EduHub-Admin.git)

## 📖 项目简介

**Tlias Web Management System** 是一套功能完整的企业/教务后台管理系统后端服务，旨在解决中小型机构中 **部门管理、员工档案、班级运营和学员信息** 四大核心业务场景的数字化难题。

系统采用 **多模块 Maven 聚合工程** 架构，将业务逻辑、数据对象、通用工具与云存储能力解耦为独立模块，并通过自研 **Aliyun OSS Spring Boot Starter** 实现开箱即用的对象存储能力。所有接口均通过 **JWT 令牌过滤器** 进行无状态鉴权，并配备 **AOP 操作日志切面**，保障系统安全可审计。

---

## 🛠️ 核心技术栈

- **框架核心**：Spring Boot 3.5.6（含 Spring MVC、Spring AOP）
- **持久层**：MyBatis 3.0.5 + MySQL 8.x
- **身份鉴权**：JJWT 0.11.5（JWT 令牌生成与校验）
- **云存储**：阿里云 OSS（自研 Spring Boot Starter 封装）
- **运行环境**：Java 21 + Maven 多模块聚合
- **部署支持**：Docker（含 Dockerfile）+ Railway 云平台适配
- **跨域处理**：Spring MVC WebMvcConfigurer CORS 全局配置

---

## 📁 目录结构说明

```
webaicode/                                  # 根聚合工程
├── tlias-parent/                           # 父工程 —— 统一管理所有依赖版本 (BOM)
├── tlias-pojo/                             # 实体层 —— 存放所有 JavaBean、VO、DTO
├── tlias-utils/                            # 工具层 —— JWT 工具、通用响应封装等
├── aliyun-oss-spring-boot-autoconfigure/   # OSS 自动配置模块 —— 实现 Bean 自动注入逻辑
├── aliyun-oss-spring-boot-starter/        # OSS Starter 模块 —— 对外发布的 Starter 坐标
└── tlias-web-management/                   # 主业务模块 (Web 服务入口)
    └── src/main/java/com/itheima/
        ├── TliasWebManagementApplication.java  # 应用启动类
        ├── controller/                     # 控制层 —— 对外暴露 REST 接口
        │   ├── DeptController.java         #   部门管理接口
        │   ├── EmpController.java          #   员工管理接口
        │   ├── ClazzController.java        #   班级管理接口
        │   ├── StudentController.java      #   学员管理接口
        │   ├── LoginController.java        #   登录/鉴权接口
        │   ├── ReportController.java       #   报表统计接口
        │   ├── StatisticsController.java   #   数据统计接口
        │   └── UploadController.java       #   文件上传接口
        ├── service/                        # 服务层 —— 核心业务逻辑
        ├── mapper/                         # 持久层 —— MyBatis Mapper 接口
        ├── filter/                         # 过滤器层 —— JWT 令牌校验过滤器
        ├── aop/                            # 切面层 —— 操作日志记录、性能监控切面
        ├── config/                         # 配置层 —— CORS 跨域全局配置
        ├── anno/                           # 自定义注解层
        └── exception/                      # 全局异常处理层
```

---

## 🚀 快速启动 (Quick Start)

### 第一步：环境准备

确保本地已安装并配置好以下环境：

| 工具 | 版本要求 |
|------|----------|
| JDK  | **21+**  |
| Maven | **3.8+** |
| MySQL | **8.0+** |
| IDE  | IntelliJ IDEA (推荐) |

### 第二步：数据库初始化

1. 在 MySQL 中创建对应数据库：

   ```sql
   CREATE DATABASE tlias CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   ```

2. 导入项目根目录下的 SQL 脚本（如有提供），或根据 `tlias-pojo` 中的实体类使用 MyBatis 自动建表。

### 第三步：修改配置文件

找到 `tlias-web-management/src/main/resources/` 下的配置文件，修改以下关键配置项：

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/tlias?useUnicode=true&characterEncoding=utf8
    username: your_username       # ← 替换为你的 MySQL 用户名
    password: your_password       # ← 替换为你的 MySQL 密码

# 阿里云 OSS 配置（用于文件上传功能）
aliyun:
  oss:
    endpoint: https://oss-cn-hangzhou.aliyuncs.com  # ← 替换为你的 OSS 地区节点
    accessKeyId: your_access_key_id                   # ← 替换为你的 AccessKeyId
    accessKeySecret: your_access_key_secret           # ← 替换为你的 AccessKeySecret
    bucketName: your_bucket_name                      # ← 替换为你的 Bucket 名称
```

### 第四步：编译与运行

```bash
# 1. 在项目根目录下，先安装所有模块到本地 Maven 仓库
mvn clean install -DskipTests

# 2. 运行主服务模块
cd tlias-web-management
mvn spring-boot:run
```

或直接在 IntelliJ IDEA 中打开并运行 `TliasWebManagementApplication.java` 主类。

服务默认监听 `http://localhost:8080` 🎉

---

## ✨ 核心特性

### 🔐 无状态 JWT 鉴权体系
通过 `TokenFilter` Servlet 过滤器实现全局令牌拦截校验。用户登录后颁发 JWT，后续所有请求（除白名单外）均须携带有效令牌，保障接口安全，天然支持水平扩展。

### 📋 AOP 操作日志记录
基于 Spring AOP 的 `OperationLogAspect` 切面，自动拦截并持久化关键业务操作记录，无需侵入业务代码，实现行为可追溯与事后审计。

### ☁️ 自研 OSS Spring Boot Starter
将阿里云 OSS 客户端的创建、配置自动装配逻辑封装为独立的 `aliyun-oss-spring-boot-starter`，任何 Spring Boot 项目只需引入该 Starter 坐标并配置参数，即可开箱即用地获得对象存储能力。

### 🌐 CORS 全局跨域配置
通过 `WebConfig` 统一配置跨域策略，支持前后端分离部署模式，前端项目可无障碍调用所有 REST 接口。

### 📊 多维度报表统计
提供班级、学员、部门、员工的多维度数据统计与报表接口，支撑管理决策可视化需求。

### 🏗️ 多模块聚合架构
严格遵循 Maven 多模块最佳实践，`tlias-parent` 统一管控所有依赖版本（BOM），各模块职责单一、边界清晰，便于独立迭代与团队协作开发。

### 🐳 容器化部署支持
项目根目录提供 `Dockerfile`，并已针对 Railway 云平台完成配置适配，可一键完成容器化构建与云端部署。

---

## 📚 附录

- 📄 **变更日志**：详见 [CHANGELOG.md](./CHANGELOG.md)
- 📜 **开源协议**：本项目遵循 [MIT License](https://opensource.org/licenses/MIT) 开源协议，欢迎学习与参考。
