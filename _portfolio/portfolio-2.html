---
title: "南方财富金融理财平台"
excerpt: "南方财富金融理财平台是基于Flutter框架开发的跨平台金融理财应用，支持Web、移动端等多平台，为用户提供社区交流、行情交易、资产管理等一站式金融服务。我在其中担任主要成员。<br/><img src='/images/portfolio2.png'>"
collection: portfolio
---
项目采用前后端分离架构，后端基于ASP.NET Core构建分层架构（Controller-Service-Repository-Domain），前端使用Flutter实现跨平台开发，支持Web与移动端双平台部署。网络层基于Dio封装JwtDio类，通过自定义JwtInterceptor拦截器实现JWT认证的自动化管理：在每次请求前自动附加Bearer令牌，当服务器返回401错误时自动触发令牌刷新流程，使用refresh token获取新令牌后自动重试原始请求，刷新失败时跳转登录页。拦截器还实现了请求队列机制，当多个请求同时遇到令牌过期时只执行一次刷新操作，其他请求排队等待，避免并发刷新导致的资源浪费。

前端使用GetIt作为依赖注入容器，集中管理SharedPreferences、TokenService及各类ApiService实例，通过AppConfigService统一管理应用配置，配合ValueNotifier实现轻量级响应式状态管理。路由管理方面，借助popupOrNavigate与Fragment组件实现响应式路由：横屏时优先使用弹窗展示子页面提升大屏设备的多任务体验，竖屏时使用标准路由切换新页面保持移动端操作习惯，底部导航栏（竖屏）和左侧导航栏（横屏）功能一致布局自适应。

系统实现了社区论坛、行情交易、个人资产管理三大核心模块，支持管理员后台（用户管理、论坛审核、统计分析、封禁历史）。错误处理通过ExceptionHandlerMiddleware拦截未处理异常，生产环境隐藏详细错误信息并记录日志。性能优化包括：使用const构造函数避免不必要的重建，ListView.builder实现懒加载，请求缓存减少重复请求，WeakReference避免内存泄漏。部署架构采用Nginx反向代理配置HTTPS和CORS策略，PostgreSQL数据库配置远程访问和定期备份，Let's Encrypt SSL证书自动续期，防火墙只开放必要端口。