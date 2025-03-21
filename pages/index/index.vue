<template>
	<view class="content">
		<uni-row>
			<uni-col :span="4">
				<uni-data-select
				  v-model="search_addr_inx"
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
			{{ search_result }}
		<uni-list>
			<uni-list-item v-for="item in search_result" :title="item.title" :note="item.note"  :thumb="item.avatar"
			 thumb-size="lg" :to="'../play_video/play_video?url=' + item.url"></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	import axios from 'axios';
	import * as cheerio from 'cheerio';
	
	export default {
		data() {
			return {
				video_info: {},
				search_addr: "",
				search_addr_inx: "tenxun",
				search_addr_obj: [
					{
						text: "爱奇艺",
						value: 'iqiyi',
						url : 'https://m.iqiyi.com'
					},
					{
						text: "腾讯视频",
						value: 'tenxun',
						url: "https://pbaccess.video.qq.com/trpc.videosearch.mobile_search.MultiTerminalSearch/MbSearch?vplatform=2",
					}
				],
				search_result: [],
			}
		},
		onLoad() {
			this.changeSource(this.search_addr_inx)
		},
		methods: {
			// 设置搜索平台
			changeSource(val) {
				for (const item of this.search_addr_obj) {
					if (item['value'] == val) {
						this.search_addr = item['url'];
					}
				}
			},
			search(item) {
				switch(this.search_addr_inx) {
					case 'iqiyi':
						this.iqiyiSearch(item['value'])
						break;
					case 'tenxun':
						this.tenxunSearch(item['value'])
						break;
				}
			},
			// 爱奇艺搜索
			iqiyiSearch(val) {
				axios({
					method: "get",
					url: this.search_addr + "/search.html?source=input&key=" + val,
					headers: {
						// 'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/134.0.0.0 Safari/537.36 Edg/134.0.0.0',
					}
				}).then(async (res) => {
					const $ = cheerio.load(res.data);
					const urls = $(".piclist-btn a")
					const titles = $('.piclist-title .c-title')
					const infos = $('.piclist-title .piclist-title-link')
					const links = $(".piclist-link")
					let obj = {}
					
					for(let inx = 0; inx < links.length; inx++ ) {
						let img_url = links.eq(inx).css('background-image').replace("url(", "");
						img_url = img_url.replace(")", "");
						obj = {
							"title" : titles.eq(inx).text(),
							"avatar" : img_url,
							"note" : infos.eq(inx).find(".c-info").text(),
							"url": this.search_addr_obj['iqiyi'] + links.eq(inx).attr('href')
						}
						
						this.search_result.push(obj)
					}
				})
			},
			
			// 腾讯视频搜索
			tenxunSearch(val) {
				uni.request({
				    url: this.search_addr,
				    data: {
						"version": "25021101",
						"clientType": 1,
						"filterValue": "",
						"uuid": "0F1FA9E0-B573-4DBD-901E-33BAD24A1AFC",
						"retry": 0,
						"query": val,
						"pagenum": 0,
						"pagesize": 30,
						"queryFrom": 2,
						"searchDatakey": "",
						"transInfo": "",
						"isneedQc": true,
						"preQid": "",
						"adClientInfo": "",
						"extraInfo": {
							"isNewMarkLabel": "1",
							"multi_terminal_pc": "1",
							"themeType": "1"
						}
					},
				    header: {
				        "Content-Type": "application/json",
				        "Origin": "https://v.qq.com",
				        "Referer": "https://v.qq.com/"
				    },
				    success: (res) => {
				        console.log(res.data);
						this.search_result = res
				    }
				});
			},
		}
	}
</script>

<style>
	.content {
		.search-input {
			
		}
		.search-btn {
			
		}
	}
</style>
