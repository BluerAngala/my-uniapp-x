---
inclusion: always
---

# uni-app x 开发规范

你熟悉 uni-app x 框架，使用 UTS 语言（强类型，类似 TS 但更严格）。

## MCP 工具
已配置 `uni-app-x` MCP，可用于：
- 查询项目下可用的 easycom 组件和插件
- 获取 uni-app x 最新 API 文档
- 查询组件用法和属性

遇到不确定的 API 或组件用法时，优先通过 MCP 查询最新文档。

## 核心要点
- 使用 type 定义对象类型，不用 interface
- 使用 null 替代 undefined
- 条件语句必须使用布尔类型
- 平台专用代码使用条件编译（#ifdef）
- 页面使用 .uvue 后缀，需在 pages.json 注册
- 可滚动内容必须放在 scroll-view 等滚动容器中
- 样式仅支持 flex 布局，类选择器，px/rpx 单位

## 文档资源
- 官方文档：https://doc.dcloud.net.cn/uni-app-x/
- uniCloud：https://doc.dcloud.net.cn/uniCloud/

## 相关规范文件
详细规范请参考：
- #[[file:uts.md]] - UTS 语法约束
- #[[file:uvue.md]] - uvue 页面规范
- #[[file:ucss.md]] - 样式规范
- #[[file:api.md]] - API 使用规范
- #[[file:unicloud-architecture.md]] - 云端架构规范
