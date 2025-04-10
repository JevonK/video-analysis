<template>
	<view>
		<div id="mui-player"></div>
		<uni-section title="播放列表" type="line">
			<scroll-view :scroll-top="scrollTop" scroll-y="true" style="height: 400px;" >
				<uni-data-checkbox mode="tag" v-model="play_url" :localdata="video_play_list" @change="change_video"></uni-data-checkbox>
			</scroll-view>
		</uni-section>
	</view>
</template>

<script>
	import 'mui-player/dist/mui-player.min.css'
	import MuiPlayer from 'mui-player';
	import Hls from 'hls.js'
	export default {
		data() {
			return {
				"scrollTop": 0,
				"video_info": {},
				"video_play_list": [],
				"play_url": "",
				"mp": null
			}
		},
		onLoad() {
			const vm = this;
			uni.getStorage({
				key: 'video_info',
				success: function(res) {
					vm.video_info = res.data
					const vod_play_url_arr = vm.video_info['vod_play_url'].split("#")
					for (const val of vod_play_url_arr) {
						const play_info = val.split("$")
						vm.video_play_list.push({
							text: play_info[0],
							value: play_info[1]
						})
					}
				}
			});

			this.$nextTick(() => {
				uni.setNavigationBarTitle({
					title: vm.video_info['vod_name']
				});
				vm.play_url = vm.video_play_list[0]['value']
				vm.play_video(vm.play_url)
			})

		},
		onUnload() {
			this.mp.destroy()
		},
		methods: {
			change_video(item) {
				this.play_video(item.detail.value)
			},
			play_video(url) {
				// 初始化 MuiPlayer 插件，MuiPlayer 方法传递一个对象，该对象包括所有插件的配置
				this.mp = new MuiPlayer({
					container: document.getElementById("mui-player"),
					src: url,
					parse: {
						type: 'hls',
						loader: Hls,
						config: { 
							debug: false,
						},
					},
					pageHead: false,
				});
			}
		}
	}
</script>

<style>

</style>