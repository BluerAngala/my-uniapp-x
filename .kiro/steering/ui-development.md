---
inclusion: always
---




# UI 组件开发规范

## tui-plus 组件库

项目使用 tui-plus 4.x 作为主要 UI 框架，组件以 `t-` 为前缀。

**官方文档**：https://life.yundie.xyz/tuiplus/docs/

### 布局组件
- `t-scroll` - 滚动容器，页面根容器必须使用
- `t-row` - 水平布局容器
- `t-col` - 垂直布局容器
- `t-card` - 卡片容器

### 原子化样式（main-class）

tui-plus 支持原子化 CSS，通过 `main-class` 属性设置样式：

```html
<!-- 布局 -->
<t-scroll main-class="h-100vh bg-#f5f5f5 p-30">
<t-row main-class="faic fjsb">  <!-- 水平居中、两端对齐 -->
<t-col main-class="fv fc">      <!-- 垂直布局、居中 -->
```

#### 常用样式缩写

| 缩写 | 含义 |
|------|------|
| `fv` | flex-direction: column（垂直布局） |
| `fh` | flex-direction: row（水平布局） |
| `fc` | 居中对齐 |
| `faic` | align-items: center |
| `fjsb` | justify-content: space-between |
| `fww` | flex-wrap: wrap |
| `w-100` | width: 100rpx |
| `h-100vh` | height: 100vh |
| `s-32` | font-size: 32rpx |
| `p-30` | padding: 30rpx |
| `m-20` | margin: 20rpx |
| `mt-10` | margin-top: 10rpx |
| `mb-30` | margin-bottom: 30rpx |
| `c-#333` | color: #333 |
| `bg-#fff` | background-color: #fff |
| `sfwb` | font-weight: bold |

### 页面基础结构

```html
<template>
  <t-scroll main-class="h-100vh bg-#f5f5f5 p-30">
    <t-card main-class="mb-30">
      <t-text main-class="s-32 sfwb c-#333 mb-20">标题</t-text>
      <t-row gap="20">
        <t-button type="primary">按钮</t-button>
      </t-row>
    </t-card>
  </t-scroll>
</template>
```

## 自定义组件开发

### Props 定义

```typescript
type ButtonProps = {
  type: string
  size: string
  disabled: boolean
}

const props = withDefaults(defineProps<ButtonProps>(), {
  type: 'default',
  size: 'medium',
  disabled: false
})
```

### 事件定义

```typescript
const emit = defineEmits<{
  (e: 'click', event: UniPointerEvent): void
  (e: 'update:modelValue', value: string): void
}>()
```

### v-model 双向绑定

```typescript
const props = defineProps<{ modelValue: string }>()
const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void
}>()

const handleInput = (e: UniInputEvent) => {
  emit('update:modelValue', e.detail.value)
}
```

### 常用事件类型

| 类型 | 用途 |
|------|------|
| `UniPointerEvent` | 点击事件 |
| `UniInputEvent` | 输入事件 |
| `UniInputFocusEvent` | 聚焦事件 |
| `UniInputBlurEvent` | 失焦事件 |
