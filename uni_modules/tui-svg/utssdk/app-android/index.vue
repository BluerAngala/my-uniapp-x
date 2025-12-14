<template>
	<view></view>
</template>
<script lang="uts">
	import FileInputStream from 'java.io.FileInputStream';
	import File from 'java.io.File';
	import SVGImageView from 'com.caverock.androidsvg.SVGImageView';
	import SVG from 'com.caverock.androidsvg.SVG';

	function getPath(path : string) : string {
		if (path.startsWith('file://')) return path.substring(7);
		return path.startsWith("/storage") ? path : UTSAndroid.convert2AbsFullPath(path)
	}

	export default {
		name: "tui-svg",
		props: {
			src: {
				type: String,
				default: ''
			},
		},
		data() {
			return {}
		},
		methods: {
			resetSource(newPath : string) {
				if (newPath.startsWith('<')) {
					this.$el?.setSVG(SVG.getFromString(newPath))
				} else {
					var path = getPath(newPath);
					if (path.startsWith('/android_asset')) {
						this.$el?.setSVG(SVG.getFromAsset(this.$androidContext!.assets, path.replace('/android_asset/', '')))
					} else if (new File(path).exists()) {
						this.$el?.setSVG(SVG.getFromInputStream(new FileInputStream(path)))
					} else {
						console.log(`svg path[${path}] not exists`)
					}
				}
			}
		},
		watch: {
			"src": {
				handler(path : string) {
					if (path != '') this.resetSource(path)
				},
				immediate: true
			}
		},
		NVLoad() : SVGImageView {
			return new SVGImageView(this.$androidContext)
		}
	}
</script>