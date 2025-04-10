<template>
	<view class="content">
		<uni-row>
			<uni-col :span="4">
				<uni-data-select
				  v-model="search_addr"
				  :localdata="search_addr_obj"
				  :clear="false"
				  @change="changeSource"
				  style="margin-top: 10px;"
				></uni-data-select>
			</uni-col>
			<uni-col :span="19">
				<uni-search-bar radius="100" placeholder="输入需要搜索的名称" clearButton="none" cancelButton="none" @confirm="search" />
			</uni-col>
		</uni-row>
		<uni-list>
			<uni-list-item v-for="item in search_result" :title="item.vod_name" :note="item.vod_content"  :thumb="item.vod_pic" :rightText="item.type_name"
			 thumb-size="lg" @click="gotoPath(item)" clickable>
				<template v-slot:body>
					<view class="uni-list-item__content uni-list-item__contentcenter">
						<text class="uni-list-item__content-title">{{ item.vod_name }}</text>
						<text class="uni-list-item__content-note">{{ item.vod_content }}</text>
					</view>
				</template>
			 </uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	import axios from 'axios';
	import * as cheerio from 'cheerio';
	
	export default {
		data() {
			return {
				proxy_url : 'https://proxy.mengze.vip/proxy/',
				hoplayer_url : 'https://hoplayer.com/index.html',
				video_info: {},
				search_addr: "https://json.heimuer.xyz",
				search_addr_obj: [
					 {
						value: 'https://json.heimuer.xyz',
						text: '黑木耳',
						detail: 'https://heimuer.tv'
					},
					{
						value: 'http://ffzy5.tv',
						text: '非凡影视',
						detail: 'http://ffzy5.tv'
					},
					 {
						value: 'https://tyyszy.com',
						text: '天涯资源',
					},
					 {
						value: 'https://www.ckzy1.com',
						text: 'CK资源',
					},
					 {
						value: 'https://360zy.com',
						text: '360资源',
					},
					 {
						value: 'https://wolongzyw.com',
						text: '卧龙资源',
					},
					 {
						value: 'https://cjhwba.com',
						text: '新华为',
					},
					 {
						value: 'https://dbzy.com',
						text: '豆瓣资源',
					}
				],
				search_result: [],
			}
		},
		onLoad() {
		},
		methods: {
			search(item) {
				console.log(item)
				const api_url = this.search_addr + "/api.php/provide/vod/?ac=videolist&wd=" + item['value']
				uni.request({
					url: this.proxy_url + encodeURIComponent(api_url),
					method:'GET',
					success: (res) => {
						const data = res.data;
						if (data.code == 1) {
							this.search_result = data.list
						} else {
							
						}
						console.log(res);
					}
				})
			},
			// 跳转页面
			gotoPath(item) {
				console.log(item)
				uni.setStorage({
					key: 'video_info',
					data: item,
					success: function () {
						uni.navigateTo({
							url: "/pages/play_video/play_video"
						})
					},
					fail: function () {
						
					}
				});
				
			}
			
		}
	}
</script>

<style lang="scss">
	.uni-list-item__content {
		display: flex;
		padding-right: 8px;
		flex: 1;
		color: #3b4144;
		flex-direction: column;
		justify-content: space-between;
		overflow: hidden;
	}
	.uni-list-item__content-title {
		font-size: 14px;
		color: #3b4144;
		overflow: hidden;
	}
	.uni-list-item__content-note {
		margin-top: 0.1875rem;
		color: #999;
		font-size: 12px;
		overflow: hidden;
		word-break:break-all;
		display:-webkit-box;
		-webkit-line-clamp:2;
		-webkit-box-orient:vertical;
	}
</style>
