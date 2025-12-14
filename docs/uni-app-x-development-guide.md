# uni-app x 多端开发入门指南

> 本文档帮助零基础开发者快速上手 uni-app x，实现一套代码同时发布到小程序、网页、App。

## 一、什么是 uni-app x？

uni-app x 是 DCloud 推出的新一代跨平台框架，特点：

- **一套代码，多端运行**：微信/支付宝小程序、H5网页、Android/iOS App
- **原生性能**：编译为原生代码，性能接近原生开发
- **强类型语言**：使用 UTS（类似 TypeScript），减少运行时错误
- **Vue 语法**：熟悉 Vue 的开发者可快速上手

## 二、开发环境准备

### 2.1 必装软件

| 软件 | 用途 | 下载地址 |
|------|------|----------|
| HBuilderX | 官方IDE，必须使用 | https://www.dcloud.io/hbuilderx.html |
| 微信开发者工具 | 调试微信小程序 | https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html |
| Chrome | 调试 H5 网页 | 自行安装 |

### 2.2 可选软件（开发 App 时需要）

| 软件 | 用途 |
|------|------|
| Android Studio | Android 真机调试 |
| Xcode（仅Mac） | iOS 真机调试 |

### 2.3 账号准备

- **DCloud 账号**：在 HBuilderX 中注册登录
- **微信小程序 AppID**：在微信公众平台申请（可先用测试号）
- **支付宝小程序 AppID**：在支付宝开放平台申请（可选）

## 三、项目结构说明

```
my-uniapp-x/
├── pages/                 # 页面目录（核心）
│   └── index/
│       └── index.uvue     # 首页
├── components/            # 公共组件（自己创建）
├── static/                # 静态资源（图片、字体等）
├── uni_modules/           # uni-app 插件
├── App.uvue               # 应用入口
├── main.uts               # 应用初始化
├── pages.json             # 页面路由配置
├── manifest.json          # 应用配置（AppID等）
└── uni.scss               # 全局样式变量
```

## 四、核心概念速览

### 4.1 页面文件 (.uvue)

每个页面由三部分组成：

```vue
<template>
  <!-- 页面结构，类似 HTML -->
  <view class="container">
    <text>{{ message }}</text>
    <button @click="handleClick">点击我</button>
  </view>
</template>

<script lang="uts">
  // 页面逻辑，使用 UTS 语言
  export default {
    data() {
      return {
        message: '你好，uni-app x！'
      }
    },
    methods: {
      handleClick() {
        this.message = '按钮被点击了'
      }
    }
  }
</script>

<style>
  /* 页面样式，仅支持 class 选择器 */
  .container {
    display: flex;
    flex-direction: column;
    padding: 20rpx;
  }
</style>
```

### 4.2 UTS vs TypeScript

UTS 是 uni-app x 的开发语言，比 TypeScript 更严格：

```typescript
// ✅ 正确写法
let name: string = '张三'
let age: number = 18
let isStudent: boolean = true

// ❌ 错误写法
let name = '张三'        // 必须声明类型
let data: undefined      // 不能用 undefined，用 null
if (name) { }            // 条件必须是 boolean，应写成 if (name != null)
```

### 4.3 常用组件对照表

| HTML | uni-app x | 说明 |
|------|-----------|------|
| `<div>` | `<view>` | 容器 |
| `<span>` | `<text>` | 文本 |
| `<img>` | `<image>` | 图片 |
| `<input>` | `<input>` | 输入框 |
| `<button>` | `<button>` | 按钮 |
| `<a>` | `<navigator>` | 跳转链接 |

### 4.4 样式注意事项

uni-app x 的样式有限制：

```css
/* ✅ 支持 */
.container { }           /* class 选择器 */
display: flex;           /* 仅支持 flex 布局 */
width: 100rpx;           /* rpx 响应式单位，750rpx = 屏幕宽度 */
width: 100px;            /* px 固定单位 */

/* ❌ 不支持 */
#id { }                  /* 不支持 ID 选择器 */
div { }                  /* 不支持标签选择器 */
float: left;             /* 不支持浮动布局 */
```

## 五、开发流程

### 5.1 新建页面

1. 在 `pages/` 目录下创建文件夹和 `.uvue` 文件
2. 在 `pages.json` 中注册页面：

```json
{
  "pages": [
    {
      "path": "pages/index/index",
      "style": {
        "navigationBarTitleText": "首页"
      }
    },
    {
      "path": "pages/list/list",
      "style": {
        "navigationBarTitleText": "列表页"
      }
    }
  ]
}
```

### 5.2 页面跳转

```typescript
// 跳转到普通页面
uni.navigateTo({
  url: '/pages/list/list'
})

// 跳转并传参
uni.navigateTo({
  url: '/pages/detail/detail?id=123'
})

// 返回上一页
uni.navigateBack()

// 跳转到 tabBar 页面
uni.switchTab({
  url: '/pages/home/home'
})
```

### 5.3 网络请求

```typescript
uni.request({
  url: 'https://api.example.com/data',
  method: 'GET',
  success: (res) => {
    console.log(res.data)
  },
  fail: (err) => {
    console.log('请求失败', err)
  }
})
```

### 5.4 数据存储

```typescript
// 存储数据
uni.setStorageSync('token', 'xxx')

// 读取数据
const token = uni.getStorageSync('token')

// 删除数据
uni.removeStorageSync('token')
```

## 六、多端运行与调试

### 6.1 运行到不同平台

在 HBuilderX 中：

| 平台 | 操作 |
|------|------|
| H5 网页 | 菜单 → 运行 → 运行到浏览器 → Chrome |
| 微信小程序 | 菜单 → 运行 → 运行到小程序模拟器 → 微信开发者工具 |
| Android | 菜单 → 运行 → 运行到手机或模拟器 → 选择设备 |
| iOS | 菜单 → 运行 → 运行到手机或模拟器 → 选择设备（需Mac） |

### 6.2 条件编译（处理平台差异）

当某些代码只在特定平台运行时：

```vue
<template>
  <!-- 仅在微信小程序显示 -->
  <!-- #ifdef MP-WEIXIN -->
  <button open-type="share">分享给好友</button>
  <!-- #endif -->
  
  <!-- 仅在 H5 显示 -->
  <!-- #ifdef H5 -->
  <button @click="copyLink">复制链接</button>
  <!-- #endif -->
</template>

<script lang="uts">
  // #ifdef MP-WEIXIN
  // 微信小程序专用代码
  // #endif
  
  // #ifdef H5
  // H5 专用代码
  // #endif
</script>
```

常用平台标识：
- `H5` - 网页
- `MP-WEIXIN` - 微信小程序
- `MP-ALIPAY` - 支付宝小程序
- `APP` - App（Android + iOS）
- `APP-ANDROID` - 仅 Android
- `APP-IOS` - 仅 iOS

## 七、发布上线

### 7.1 发布 H5 网页

1. HBuilderX 菜单 → 发行 → 网站-H5手机版
2. 生成的文件在 `unpackage/dist/build/h5`
3. 部署到服务器或 Netlify/Vercel

### 7.2 发布微信小程序

1. HBuilderX 菜单 → 发行 → 小程序-微信
2. 在微信开发者工具中点击"上传"
3. 登录微信公众平台提交审核

### 7.3 发布 App

1. HBuilderX 菜单 → 发行 → 原生App-云打包
2. 配置证书（Android 需要签名证书，iOS 需要苹果开发者账号）
3. 等待云端打包完成，下载安装包

## 八、推荐学习路径

```
第1周：熟悉 HBuilderX，跑通 H5 和小程序
    ↓
第2周：学习 UTS 语法，掌握组件和样式
    ↓
第3周：学习页面跳转、网络请求、数据存储
    ↓
第4周：开发一个完整的小项目（如待办清单）
    ↓
第5周：学习条件编译，处理多端差异
    ↓
第6周：尝试打包发布到各平台
```

## 九、常见问题

### Q1：为什么样式不生效？
- 检查是否使用了 class 选择器
- 检查是否使用了 flex 布局
- 检查单位是否正确（rpx/px）

### Q2：为什么代码报错？
- UTS 要求所有变量声明类型
- 不能使用 undefined，用 null 代替
- 条件判断必须是 boolean 类型

### Q3：如何调试？
- 使用 `console.log()` 打印日志
- H5 用 Chrome 开发者工具
- 小程序用微信开发者工具的调试面板

### Q4：组件找不到？
- 检查 `pages.json` 是否注册了页面
- 检查组件路径是否正确
- 使用 easycom 自动导入组件

## 十、参考资源

- [uni-app x 官方文档](https://doc.dcloud.net.cn/uni-app-x/)
- [UTS 语法文档](https://doc.dcloud.net.cn/uni-app-x/uts/)
- [组件文档](https://doc.dcloud.net.cn/uni-app-x/component/)
- [API 文档](https://doc.dcloud.net.cn/uni-app-x/api/)
- [插件市场](https://ext.dcloud.net.cn/)

---

> 💡 提示：遇到问题时，优先查阅官方文档，或在 DCloud 社区提问。
