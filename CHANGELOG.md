# Changelog

本文件记录了 **Tlias Web Management System** 项目所有值得关注的变更。

格式遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.0.0/) 规范，
版本号遵循 [语义化版本](https://semver.org/lang/zh-CN/) 规范。

---

## [1.1.0] - 2026-04-04

### Added

- **自研 Aliyun OSS Spring Boot Starter**：将阿里云对象存储能力封装为独立的 `aliyun-oss-spring-boot-starter`，任何 Spring Boot 项目引入该 Starter 后，仅需在 `application.yml` 中配置 OSS 参数即可自动注入客户端，无需手动编写 Bean 配置。
- **AOP 操作日志切面**：新增 `OperationLogAspect`，通过切面自动记录关键业务操作至数据库，实现操作行为的全链路可追溯与事后审计，无需修改任何业务代码。
- **全局 CORS 跨域配置**：新增 `WebConfig`，统一处理所有接口的跨域请求，开箱即可支持前后端分离部署架构。
- **Docker 容器化支持**：在项目根目录新增 `Dockerfile`，支持将应用打包为 Docker 镜像，并完成对 Railway 云平台的环境配置适配，实现云端一键部署。

### Changed

- **多模块架构重构**：将原单体工程拆分为 Maven 多模块聚合项目，引入独立的 `tlias-parent`（版本管理）、`tlias-pojo`（实体层）、`tlias-utils`（工具层）模块，各模块职责单一，解耦效果显著。
- **JWT 工具类升级**：对令牌生成与解析工具类进行了重构，统一配置 Token 有效期与签名密钥，并修复了在特定场景下签名校验不一致的问题。
- **Maven 构建配置优化**：规范化根聚合 POM 与父工程 POM 的分工，`tlias-parent` 专注于依赖版本锁定（`dependencyManagement`），根 POM 专注于模块聚合（`modules`），消除循环依赖风险。

### Fixed

- **修复 Maven 多模块构建失败问题**：由于模块间依赖声明顺序错误，导致本地 `mvn install` 时出现 `Could not find artifact` 错误。已在根 POM 的 `<modules>` 中按依赖拓扑顺序重新排列模块，确保被依赖模块优先构建。

---

## [1.0.0] - 2026-03-01

### Added

- 完成员工管理系统核心功能的初始版本开发，包含部门管理（CRUD）、员工管理（分页查询、条件筛选）等基础业务接口。
- 集成 JWT 鉴权机制，实现基于 `TokenFilter` 的接口统一拦截与身份校验。
- 完成班级管理与学员管理模块，支持学员信息的录入、查询与统计。
- 提供文件上传接口，对接阿里云 OSS 实现头像等静态资源的云端存储。
- 实现报表统计接口，可按多维度聚合业务数据，为管理端可视化提供数据支撑。
