
# Svg 组件
渲染 Svg 图标，支持配置图标来源等


## 支持平台
 
| 安卓	| ios	| web	| 微信小程序	| 支付宝小程序	| QQ小程序	|
| :----:| :----:| :----:| :----:	| :----:		| :----:	|
| √	| √		| √		| √			| x				| x			|

## 示例

```vue
<template>
	<t-page title='svg' main-class='p-30'>
		<t-card title="Svg" sub-title="支持安卓,IOS,web-支持网络路径和字符串格式" main-class="mb-30"></t-card>
		<t-section title="字符串" main-class="mb-30"></t-section>
		<t-col main-class="tdr tdb mb-30 p-30">
			<t-svg :src="src" main-class="twh-50" />
		</t-col>
		<t-section title="网络" main-class="mb-30"></t-section>
		<t-col main-class="tdr tdb mb-30 p-30">
			<t-svg src="https://life.yundie.xyz/tuiplus/static/svg/adafruit.svg" main-class="twh-50" />
		</t-col>
		<t-section title="路径" main-class="mb-30"></t-section>
		<t-col main-class="tdr tdb mb-30 p-30">
			<t-svg src="/pagesA/static/svg/adafruit.svg" main-class="twh-50" />
		</t-col>
	</t-page>
</template>

<script>
	export default {
		data() {
			return {
				// XML资源
				src: '<?xml version="1.0" encoding="UTF-8"?><svg width="24" height="24" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg"><circle cx="12" cy="24" r="3" fill="#333"/><circle cx="24" cy="24" r="3" fill="#333"/><circle cx="36" cy="24" r="3" fill="#333"/></svg>'
			}
		},
		onLoad() {

		}
	}
</script>
```

## Props
| 名称 | 描述 | 类型 | 默认值 | 可选值 |
| :----- | :------------------- | :----- | :----- | :----- |
| src | Svg 图标的来源 | String | '' | - |

## Events
| 事件名| 描述				| 回调参数	| 版本	|
| :-----| :---------------	| :-----:	| :---:	|
| -		| -					| -			| -		|

## Slots
| 名称 | 描述 |
| :----- | :------------- |
| - | - |


