<template>
	<view>
		<uni-data-select
		  v-model="resolution_addr_inx"
		  :localdata="resolution_addr_list"
		  @change="changeAddr"
		></uni-data-select>
		      
		<web-view style="width: 100%;height: 90%;margin-top: 3%;" :src="analysis_url"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				"resolution_addr_inx" : 1,
				"resolution_addr_list" :[
				    {
						"value" : 0,
				        "text": "线路一",
				        "url": "https://jx.nnxv.cn/tv.php?url="
				    },
				    {
						"value" : 1,
				        "text": "线路二",
				        "url": "https://jx.m3u8.tv/jiexi/?url="
				    },
				    {
						"value" : 2,
				        "text": "线路三",
				        "url": "https://jx.xmflv.com/?url="
				    },
				    {
						"value" : 3,
				        "text": "线路四",
				        "url": "http://www.pangujiexi.cc/jiexi.php?url="
				    },
				    {
						"value" : 4,
				        "text": "线路五",
				        "url": "https://jx.mmkv.cn/tv.php?url="
				    },
				    {
						"value" : 5,
				        "text": "线路六",
				        "url": "https://jx.playerjy.com/?url="
				    }
				],
				"video_info": {},
				"video_url": "",
				"analysis_url": "",
			}
		},
		onLoad(option) {
			this.video_url = option.url
			// 获取视频信息
			this.getVideoInfo(option.url)
		},
		methods: {
			getVideoInfo(url) {
				this.analysis_url = this.resolution_addr_list[this.resolution_addr_inx]['url'] + this.video_url;
			},
			changeAddr(val) {
				this.analysis_url = this.resolution_addr_list[val]['url'] + this.video_url;
			}
		}
	}
</script>

<style>

</style>
