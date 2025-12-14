<template>
	<view>
		<img :src="resetSource"></img>
	</view>
</template>

<script lang="uts">
	/**
	 * tui-svg svg组件
	 * @description
	 * @tutorial 
	 * @property {String} src svg资源参数
	 */
	export default {
		computed: {
			resetSource() : string {
				if (this.src.startsWith('<')) {
					return `data:image/svg+xml;charset=utf-8,${encodeURIComponent(this.src)}`
				} else {
					return this.src
				}
			}
		},
		props: {
			src: {
				type: String,
				default: ""
			}
		}
	}
</script>