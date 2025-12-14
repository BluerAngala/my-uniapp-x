<template>
	<view></view>
</template>
<script lang="uts">
	import { SVGKImage } from "SVGKit"  assert { type: "implementationOnly" };
	import { UIView, UIImageView, UIColor, UIImageView } from "UIKit"
	import { URL, Data } from 'Foundation'
	export default {
		name: "tui-svg",
		props: {
			src: {
				type: String,
				default: ''
			},
		},
		data() {
			return {
			}
		},
		methods: {
			getPath(path : string) : string {
				if (path.hasPrefix('file://')) return path.substring(7)
				if (path.hasPrefix("/var/")) return path
				return UTSiOS.convert2AbsFullPath(path)
			},
			resetSource(newPath : string) {
				if (newPath.hasPrefix('<')) {
					try {
						let fileData : Data = newPath.data(using = String.Encoding.utf8)!
						this.$el.image = new SVGKImage(data = fileData).uiImage
					} catch (err) {
						console.log(err)
					}
				} else {
					try {
						let path = this.getPath(newPath)
						this.$el.image = new SVGKImage(contentsOfFile = path).uiImage
					} catch (err) {
						console.log(err)
					}
				}
			}
		},
		watch: {
			"src": {
				handler(path : string, _ : string) {
					this.resetSource(path)
				},
				immediate: false
			}
		},
		// 创建组件对应的原生 View，返回值类型需要修改为实际 View 类型
		NVLoad() : UIImageView {
			return new UIImageView()
		},
		NVLoaded() {
			this.resetSource(this.src)
		}
	}
</script>