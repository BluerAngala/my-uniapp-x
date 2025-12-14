# Design Document

## Overview

cc-ui 是一个基于 tui-plus 二次开发的 uni-app x UI 组件库插件。采用 Vue3 Composition API + UTS 语言开发，支持 AutoStyle 简写样式系统，目标是打造一套简洁、高效、易用的跨平台 UI 组件库。

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                      应用层                              │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐    │
│  │ 登录页  │  │ 列表页  │  │ 详情页  │  │ 个人中心 │    │
│  └────┬────┘  └────┬────┘  └────┬────┘  └────┬────┘    │
│       │            │            │            │          │
├───────┴────────────┴────────────┴────────────┴──────────┤
│                    业务组件层                            │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │
│  │ biz-user-card│  │biz-goods-card│  │biz-order-card│    │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘      │
│         │                │                │              │
├─────────┴────────────────┴────────────────┴─────────────┤
│                    cc-ui 组件层                          │
│  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐│
│  │cc-button│ │cc-input│ │cc-card │ │cc-popup│ │cc-tabs ││
│  └────┬───┘ └────┬───┘ └────┬───┘ └────┬───┘ └────┬───┘│
│       │          │          │          │          │     │
├───────┴──────────┴──────────┴──────────┴──────────┴─────┤
│                    公共模块层                            │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐                  │
│  │  props  │  │  utils  │  │  style  │                  │
│  └─────────┘  └─────────┘  └─────────┘                  │
└─────────────────────────────────────────────────────────┘
```

## Components and Interfaces

### 公共 Props 接口

所有组件继承的公共属性：

```typescript
// common/props/index.uts
type CommonProps = {
  mainClass: string      // AutoStyle 简写样式
  nativeClass: string    // 原生 class
  disabled: boolean      // 是否禁用
  stop: boolean          // 是否阻止事件冒泡
}
```

### 组件接口设计

#### cc-button 按钮组件

```typescript
type ButtonProps = {
  type: 'default' | 'primary' | 'success' | 'warning' | 'danger'
  size: 'large' | 'medium' | 'small' | 'mini'
  loading: boolean
  disabled: boolean
  mainClass: string
}

// 事件
emit('click', event: UniPointerEvent)
```

#### cc-input 输入框组件

```typescript
type InputProps = {
  modelValue: string
  type: 'text' | 'number' | 'password' | 'tel'
  placeholder: string
  maxlength: number
  disabled: boolean
  mainClass: string
}

// 事件
emit('update:modelValue', value: string)
emit('input', event: UniInputEvent)
emit('focus', event: UniFocusEvent)
emit('blur', event: UniBlurEvent)
```

#### cc-popup 弹出层组件

```typescript
type PopupProps = {
  modelValue: boolean
  position: 'top' | 'bottom' | 'left' | 'right' | 'center'
  mask: boolean
  maskClosable: boolean
  mainClass: string
}

// 事件
emit('update:modelValue', value: boolean)
emit('open')
emit('close')
```

## Data Models

### 组件状态管理

组件内部状态使用 Vue3 响应式 API：

```typescript
// 使用 ref 管理简单状态
const isLoading = ref<boolean>(false)

// 使用 reactive 管理复杂状态
const state = reactive<{
  visible: boolean
  position: string
}>({
  visible: false,
  position: 'bottom'
})

// 使用 computed 派生状态
const buttonClass = computed(() => {
  return `cc-button cc-button--${props.type} cc-button--${props.size}`
})
```

## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system-essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*

### Property 1: 组件 Props 默认值一致性
*For any* cc-ui 组件，当未传入可选 props 时，组件 SHALL 使用预定义的默认值正常渲染
**Validates: Requirements 2.1, 2.2, 2.3, 2.4, 2.5**

### Property 2: 双向绑定数据同步
*For any* 支持 v-model 的组件（cc-input、cc-popup、cc-tabs），当内部状态变化时，SHALL 正确触发 update:modelValue 事件
**Validates: Requirements 3.1, 5.3, 6.2, 6.3**

### Property 3: 事件冒泡控制
*For any* cc-ui 组件，当 stop 属性为 true 时，点击事件 SHALL 不向父组件冒泡
**Validates: Requirements 2.3**

### Property 4: 禁用状态行为
*For any* 支持 disabled 属性的组件，当 disabled 为 true 时，组件 SHALL 不响应用户交互
**Validates: Requirements 2.3, 3.1**

## Error Handling

### 组件错误处理策略

1. **Props 类型校验**: 使用 UTS 强类型确保 props 类型正确
2. **默认值兜底**: 所有可选 props 提供合理默认值
3. **边界条件处理**: 空数据、异常输入等情况的优雅降级

```typescript
// 示例：图片加载错误处理
const handleImageError = (e: UniImageErrorEvent) => {
  console.log('图片加载失败', e.detail)
  emit('error', e)
}
```

## Testing Strategy

### 单元测试

- 测试每个组件的 props 默认值
- 测试事件触发是否正确
- 测试禁用状态下的行为

### 集成测试

- 测试组件组合使用场景
- 测试表单提交流程
- 测试页面模板完整功能

### 手动测试

- 在 HBuilderX 中运行到不同平台（H5、微信小程序、Android）
- 验证样式一致性
- 验证交互体验

## File Structure

```
uni_modules/cc-ui/
├── components/
│   ├── cc-view/
│   │   └── cc-view.uvue
│   ├── cc-text/
│   │   └── cc-text.uvue
│   ├── cc-button/
│   │   └── cc-button.uvue
│   ├── cc-icon/
│   │   └── cc-icon.uvue
│   ├── cc-image/
│   │   └── cc-image.uvue
│   ├── cc-input/
│   │   └── cc-input.uvue
│   ├── cc-form/
│   │   └── cc-form.uvue
│   ├── cc-form-item/
│   │   └── cc-form-item.uvue
│   ├── cc-row/
│   │   └── cc-row.uvue
│   ├── cc-col/
│   │   └── cc-col.uvue
│   ├── cc-card/
│   │   └── cc-card.uvue
│   ├── cc-cell/
│   │   └── cc-cell.uvue
│   ├── cc-loading/
│   │   └── cc-loading.uvue
│   ├── cc-empty/
│   │   └── cc-empty.uvue
│   ├── cc-popup/
│   │   └── cc-popup.uvue
│   ├── cc-navbar/
│   │   └── cc-navbar.uvue
│   ├── cc-tabbar/
│   │   └── cc-tabbar.uvue
│   └── cc-tabs/
│       └── cc-tabs.uvue
├── common/
│   ├── props/
│   │   └── index.uts
│   ├── utils/
│   │   └── index.uts
│   └── style/
│       └── index.uts
├── static/
│   └── icons/
├── index.uts
├── package.json
├── readme.md
└── changelog.md
```
