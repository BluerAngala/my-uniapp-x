# uni-app x 练手项目设计方案

## 一、项目目标

基于 **tui-plus** UI 框架进行二次开发，构建一套可复用的 uni-app x 开发模板，包含：
- 基于 tui-plus 的业务组件扩展
- 常用页面模板
- 云端开发示例（云对象、云函数）
- 开发规范和最佳实践

## 技术栈

| 技术 | 说明 |
|------|------|
| uni-app x | 跨平台框架 |
| UTS | 强类型语言（类似 TypeScript） |
| Vue3 Composition API | 组件开发方式 |
| tui-plus 4.x | 参考的 UI 组件库 |
| uniCloud | 云端开发 |

## 开发环境

| 环境 | 说明 |
|------|------|
| 操作系统 | Windows 11 |
| IDE | HBuilderX |
| 命令行工具 | Git Bash |
| 包管理器 | pnpm |
| 版本控制 | Git + GitHub CLI |

## 关于 tui-plus

tui-plus 是专为 uni-app x 设计的 UI 框架，特点：
- ✅ 140+ 组件，覆盖常见场景
- ✅ 纯 UTS 实现，原生性能
- ✅ Vue3 Composition API
- ✅ AutoStyle 简写样式系统
- ✅ 内置工具库（加密、日期、颜色等）
- ✅ 支持暗黑模式、国际化

文档地址：https://life.yundie.xyz/tuiplus/docs/

## 二、目录结构设计

```
my-uniapp-x/
├── components/                    # 业务组件（基于 tui-plus 扩展）
│   └── biz/                       # 业务组件
│       ├── biz-login-form/        # 登录表单
│       ├── biz-user-card/         # 用户卡片
│       ├── biz-goods-card/        # 商品卡片
│       ├── biz-order-card/        # 订单卡片
│       └── biz-comment-item/      # 评论项
│
├── pages/                         # 页面
│   ├── index/                     # 首页（组件导航）
│   └── template/                  # 页面模板
│       ├── login/                 # 登录页
│       ├── register/              # 注册页
│       ├── user-center/           # 个人中心
│       ├── settings/              # 设置页
│       ├── list/                  # 列表页
│       ├── detail/                # 详情页
│       ├── search/                # 搜索页
│       └── result/                # 结果页
│
├── api/                           # 接口定义
│   └── user.uts                   # 用户相关接口
│
├── types/                         # 类型定义
│   └── user.uts                   # 用户类型
│
├── uni_modules/                   # 插件目录
│   ├── tui-plus/                  # tui-plus UI 框架（已集成）
│   ├── tui-scan/                  # 扫码插件
│   ├── tui-schema/                # Schema 插件
│   └── tui-svg/                   # SVG 插件
│
├── uniCloud-alipay/               # 云端代码（阿里云）
│   ├── cloudfunctions/            # 云函数/云对象
│   └── database/                  # 数据库
│
├── docs/                          # 项目文档
├── App.uvue                       # 应用入口
├── main.uts                       # 应用初始化
├── pages.json                     # 页面配置
└── manifest.json                  # 应用配置
```

**说明：** 基础 UI 组件直接使用 tui-plus 提供的 `t-xxx` 组件，我们只需开发业务组件和页面模板。

## 三、tui-plus AutoStyle 简写规则

tui-plus 提供了强大的 AutoStyle 简写系统，通过 `main-class` 属性传入简写样式：

### 3.1 常用简写速查表

| 简写 | 完整样式 | 说明 |
|------|----------|------|
| `fl` | `display: flex` | flex 布局 |
| `fv` | `flex-direction: column` | 纵向排列 |
| `fc` | `align-items: center; justify-content: center` | 居中 |
| `faic` | `align-items: center` | 交叉轴居中 |
| `fjcc` | `justify-content: center` | 主轴居中 |
| `fjcb` | `justify-content: space-between` | 两端对齐 |
| `f-1` | `flex: 1` | 占满剩余空间 |
| `p-20` | `padding: 20rpx` | 内边距 |
| `px-20` | `padding-left/right: 20rpx` | 水平内边距 |
| `py-20` | `padding-top/bottom: 20rpx` | 垂直内边距 |
| `m-20` | `margin: 20rpx` | 外边距 |
| `mt-20` | `margin-top: 20rpx` | 上外边距 |
| `mb-20` | `margin-bottom: 20rpx` | 下外边距 |
| `ml-20` | `margin-left: 20rpx` | 左外边距 |
| `mr-20` | `margin-right: 20rpx` | 右外边距 |
| `w-200` | `width: 200rpx` | 宽度 |
| `h-100` | `height: 100rpx` | 高度 |
| `twh-80` | `width: 80rpx; height: 80rpx` | 宽高相等 |
| `r-20` | `border-radius: 20rpx` | 圆角 |
| `s-28` | `font-size: 28rpx` | 字号 |
| `sfwb` | `font-weight: bold` | 粗体 |
| `c-#333` | `color: #333` | 文字颜色 |
| `bg-#fff` | `background-color: #fff` | 背景色 |
| `oh` | `overflow: hidden` | 隐藏溢出 |
| `ov` | `overflow: visible` | 显示溢出 |

### 3.2 使用示例

```vue
<template>
  <!-- 使用 tui-plus 组件 + AutoStyle -->
  <t-view main-class="fl fv p-20 bg-#f5f5f5">
    <t-text main-class="s-32 c-#333 sfwb">标题文字</t-text>
    <t-text main-class="s-24 c-#999 mt-10">描述文字</t-text>
  </t-view>
  
  <!-- 卡片示例 -->
  <t-card main-class="m-20 r-16">
    <t-row main-class="faic fjcb">
      <t-avatar src="/static/avatar.png" main-class="twh-80" />
      <t-text main-class="f-1 ml-20 s-28">用户名</t-text>
      <t-icon name="arrow-right" />
    </t-row>
  </t-card>
  
  <!-- 按钮示例 -->
  <t-button type="primary" main-class="m-20">
    提交
  </t-button>
</template>
```

### 3.3 tui-plus 常用组件速查

| 组件 | 说明 | 常用属性 |
|------|------|----------|
| `t-view` | 视图容器 | main-class |
| `t-text` | 文本 | main-class |
| `t-row` | 横向布局 | main-class |
| `t-col` | 纵向布局 | main-class |
| `t-button` | 按钮 | type, size, loading |
| `t-input` | 输入框 | v-model, placeholder, type |
| `t-icon` | 图标 | name, main-class |
| `t-image` | 图片 | src, mode |
| `t-avatar` | 头像 | src, main-class |
| `t-card` | 卡片 | main-class |
| `t-cell` | 单元格 | title, value, arrow |
| `t-list` | 列表 | - |
| `t-list-item` | 列表项 | title, value |
| `t-popup` | 弹出层 | v-model, position |
| `t-dialog` | 对话框 | v-model, title |
| `t-loading` | 加载 | name |
| `t-empty` | 空状态 | text |
| `t-tabs` | 标签页 | v-model |
| `t-search` | 搜索框 | v-model, placeholder |
| `t-navbar` | 导航栏 | title |
| `t-tabbar` | 底部导航 | v-model |
| `t-form` | 表单 | - |
| `t-form-item` | 表单项 | label, required |
| `t-scroll` | 滚动容器 | refresher |

## 四、业务组件设计

### 4.1 组件命名规范

- 组件文件夹：`biz-组件名`（如 `biz-user-card`）
- 组件文件：`biz-组件名.uvue`
- 使用时：`<biz-user-card>` （easycom 自动导入）

### 4.2 业务组件代码模板

```vue
<!-- components/biz/biz-user-card/biz-user-card.uvue -->
<template>
  <t-card main-class="m-20" @click="handleClick">
    <t-row main-class="faic">
      <t-avatar :src="avatar" main-class="twh-80" />
      <t-col main-class="f-1 ml-20">
        <t-text main-class="s-32 c-#333 sfwb">{{ nickname }}</t-text>
        <t-text main-class="s-24 c-#999 mt-8">{{ description }}</t-text>
      </t-col>
      <t-icon name="arrow-right" main-class="c-#ccc" />
    </t-row>
  </t-card>
</template>

<script setup lang="uts">
  /**
   * biz-user-card 用户卡片组件
   * @description 展示用户头像、昵称、描述的卡片
   */
  
  type UserCardProps = {
    avatar: string
    nickname: string
    description: string
  }
  
  const props = withDefaults(defineProps<UserCardProps>(), {
    avatar: '',
    nickname: '',
    description: ''
  })
  
  const emit = defineEmits<{
    (e: 'click'): void
  }>()
  
  const handleClick = () => {
    emit('click')
  }
</script>
```

## 五、页面模板设计

### 5.1 列表页模板（下拉刷新 + 上拉加载）

```vue
<!-- pages/template/list/list.uvue -->
<template>
  <t-scroll 
    main-class="h-100vh bg-#f5f5f5"
    :refresher="true"
    :refresher-triggered="isRefreshing"
    @refresherrefresh="onRefresh"
    @scrolltolower="onLoadMore"
  >
    <!-- 列表内容 -->
    <t-view main-class="p-20">
      <t-card 
        v-for="(item, index) in list" 
        :key="index"
        main-class="mb-20"
      >
        <t-text>{{ item.title }}</t-text>
      </t-card>
    </t-view>
    
    <!-- 加载状态 -->
    <t-view main-class="fc p-30">
      <t-loading v-if="isLoading" />
      <t-text v-else-if="noMore" main-class="s-24 c-#999">没有更多了</t-text>
    </t-view>
    
    <!-- 空状态 -->
    <t-empty v-if="list.length == 0 && !isLoading" text="暂无数据" />
  </t-scroll>
</template>

<script setup lang="uts">
  import { ref } from 'vue'
  
  type ListItem = {
    id: string
    title: string
  }
  
  const list = ref<ListItem[]>([])
  const page = ref<number>(1)
  const pageSize = ref<number>(10)
  const isRefreshing = ref<boolean>(false)
  const isLoading = ref<boolean>(false)
  const noMore = ref<boolean>(false)
  
  const loadData = async () => {
    if (isLoading.value) return
    isLoading.value = true
    
    try {
      // TODO: 调用接口
      const res = await fetchList(page.value, pageSize.value)
      
      if (page.value == 1) {
        list.value = res.list
      } else {
        list.value = list.value.concat(res.list)
      }
      
      noMore.value = res.list.length < pageSize.value
    } catch (e) {
      console.log('加载失败', e)
    } finally {
      isLoading.value = false
      isRefreshing.value = false
    }
  }
  
  const onRefresh = () => {
    isRefreshing.value = true
    page.value = 1
    noMore.value = false
    loadData()
  }
  
  const onLoadMore = () => {
    if (noMore.value || isLoading.value) return
    page.value++
    loadData()
  }
  
  // 模拟接口
  const fetchList = (p: number, size: number): Promise<{ list: ListItem[] }> => {
    return new Promise((resolve) => {
      setTimeout(() => {
        const result: ListItem[] = []
        for (let i = 0; i < size; i++) {
          result.push({
            id: `${p}-${i}`,
            title: `列表项 ${(p - 1) * size + i + 1}`
          })
        }
        resolve({ list: result })
      }, 1000)
    })
  }
  
  onLoad(() => {
    loadData()
  })
</script>
```

### 5.2 登录页模板

```vue
<!-- pages/template/login/login.uvue -->
<template>
  <t-view main-class="fv p-60 bg-#fff h-100vh">
    <!-- Logo -->
    <t-view main-class="fc fv mb-80">
      <t-image src="/static/logo.png" main-class="twh-160 mb-30" mode="aspectFit" />
      <t-text main-class="s-40 sfwb c-#333">欢迎登录</t-text>
    </t-view>
    
    <!-- 表单 -->
    <t-form>
      <t-form-item label="手机号">
        <t-input 
          v-model="form.phone" 
          placeholder="请输入手机号"
          type="number"
          maxlength="11"
        />
      </t-form-item>
      <t-form-item label="密码">
        <t-input 
          v-model="form.password" 
          placeholder="请输入密码"
          type="password"
        />
      </t-form-item>
      <t-button type="primary" size="large" main-class="mt-40" @click="handleLogin">
        登录
      </t-button>
    </t-form>
    
    <!-- 其他操作 -->
    <t-row main-class="fjcb mt-40">
      <t-text main-class="s-28 c-#007aff" @click="goRegister">注册账号</t-text>
      <t-text main-class="s-28 c-#007aff" @click="goForget">忘记密码</t-text>
    </t-row>
  </t-view>
</template>

<script setup lang="uts">
  import { reactive } from 'vue'
  
  type LoginForm = {
    phone: string
    password: string
  }
  
  const form = reactive<LoginForm>({
    phone: '',
    password: ''
  })
  
  const handleLogin = () => {
    if (form.phone.length != 11) {
      uni.showToast({ title: '请输入正确的手机号', icon: 'none' })
      return
    }
    if (form.password.length < 6) {
      uni.showToast({ title: '密码至少6位', icon: 'none' })
      return
    }
    
    uni.showLoading({ title: '登录中...' })
    // TODO: 调用云对象登录
  }
  
  const goRegister = () => {
    uni.navigateTo({ url: '/pages/template/register/register' })
  }
  
  const goForget = () => {
    uni.showToast({ title: '功能开发中', icon: 'none' })
  }
</script>
```

## 六、云端开发设计

### 6.1 云对象模板

```javascript
// uniCloud-alipay/cloudfunctions/user-center/index.obj.js

const db = uniCloud.database()
const usersCollection = db.collection('users')

module.exports = {
  _before: async function() {
    // 公共逻辑：验证 token
    const token = this.getHttpInfo().headers['x-token']
    if (this.getMethodName() !== 'login' && this.getMethodName() !== 'register') {
      if (!token) {
        throw new Error('请先登录')
      }
    }
  },
  
  // 用户登录
  async login(phone, password) {
    if (!phone || !password) {
      return { code: 400, msg: '参数不完整' }
    }
    
    const userRes = await usersCollection.where({ phone }).get()
    if (userRes.data.length === 0) {
      return { code: 404, msg: '用户不存在' }
    }
    
    const user = userRes.data[0]
    if (user.password !== password) {
      return { code: 401, msg: '密码错误' }
    }
    
    const token = Date.now().toString()
    
    return {
      code: 0,
      msg: '登录成功',
      data: {
        token,
        userInfo: {
          _id: user._id,
          phone: user.phone,
          nickname: user.nickname,
          avatar: user.avatar
        }
      }
    }
  },
  
  // 用户注册
  async register(phone, password, nickname) {
    if (!phone || !password) {
      return { code: 400, msg: '参数不完整' }
    }
    
    const existRes = await usersCollection.where({ phone }).count()
    if (existRes.total > 0) {
      return { code: 409, msg: '手机号已注册' }
    }
    
    const result = await usersCollection.add({
      phone,
      password,
      nickname: nickname || `用户${phone.slice(-4)}`,
      avatar: '',
      createTime: Date.now()
    })
    
    return { code: 0, msg: '注册成功', data: { _id: result.id } }
  }
}
```

### 6.2 前端调用云对象

```typescript
// api/user.uts

const userCenter = uniCloud.importObject('user-center')

export async function login(phone: string, password: string) {
  return await userCenter.login(phone, password)
}

export async function register(phone: string, password: string, nickname: string) {
  return await userCenter.register(phone, password, nickname)
}
```

## 七、开发计划

### 第一阶段：基础搭建
- [x] 创建项目目录结构
- [x] 集成 tui-plus UI 框架
- [ ] 熟悉 tui-plus 组件和 AutoStyle 用法

### 第二阶段：页面模板开发（含业务组件 + 云端配合）

**用户模块**
- [ ] 登录页（含云对象：用户登录）
- [ ] 注册页（含云对象：用户注册）
- [ ] 个人中心页（含云对象：获取用户信息）
- [ ] 设置页
- [ ] 编辑资料页（含云对象：更新用户信息）

**通用模块**
- [ ] 列表页模板（下拉刷新 + 上拉加载 + 分页）
- [ ] 详情页模板
- [ ] 搜索页模板
- [ ] 结果页模板（成功/失败）
- [ ] 关于页模板
- [ ] 协议页模板（用户协议、隐私政策）

**业务组件（开发页面过程中提取）**
- [ ] biz-user-card 用户卡片
- [ ] biz-goods-card 商品卡片
- [ ] biz-order-card 订单卡片
- [ ] biz-comment-item 评论项

### 第三阶段：完善文档
- [ ] 组件使用文档
- [ ] 页面模板使用文档
- [ ] 云端接口文档

---

> 目标：基于 tui-plus 框架，快速开发业务组件和页面模板。
