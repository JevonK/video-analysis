<template>
	<view class="content">
			<uni-search-bar radius="100" placeholder="输入需要搜索的名称" clearButton="none" cancelButton="none" @confirm="search" />
			
			<template v-for="item in search_result">
				<uni-list-chat 
				:avatar-circle="true" 
				:title="item.title" 
				:avatar="item.avatar" 
				:note="item.note" 
				time="" 
				></uni-list-chat>
			</template>
	</view>
</template>

<script>
	import axios from 'axios';
	import * as cheerio from 'cheerio';
	
	export default {
		data() {
			return {
				video_info: {},
				search_addr_obj: {
					'iqiyi' : 'https://m.iqiyi.com'
				},
				search_url: '',
				search_result: [],
			}
		},
		onLoad() {

		},
		methods: {
			changeVideo(val) {
				
			},
			search(item) {
				this.search_url = ""
				this.search_url = this.search_addr_obj['iqiyi'] + "/search.html?source=input&key=" + item['value'];
				console.log(this.search_url)
				
				axios({
					method: "get",
					url: this.search_url,
					headers: {
						'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/134.0.0.0 Safari/537.36 Edg/134.0.0.0',
					}
				}).then(async (res) => {
					// console.log(res.data);
					// const data = res.data
					const $ = cheerio.load(res.data);
					const urls = $(".piclist-btn a")
					const titles = $('.piclist-title .c-title')
					const infos = $('.piclist-title .piclist-title-link')
					const links = $(".piclist-link")
					let obj = {}
					
					for(let inx = 0; inx < links.length; inx++ ) {
						let img_url = links.eq(inx).css('background-image').replace("url(", "");
						img_url = img_url.replace(")", "");
						await axios.get(img_url).then(res => {
							img_url = res.data
						})
						obj = {
							"title" : titles.eq(inx).text(),
							"avatar" : img_url,
							"avatar":"http://pic1.iqiyipic.com/image/20250110/0f/eb/v_112869373_m_601_m3.jpg",
							"note" : infos.eq(inx).find(".c-info").text(),
							"url": this.search_addr_obj['iqiyi'] + links.eq(inx).attr('href')
						}
						
						this.search_result.push(obj)
 					}
					console.log("数据参数:",this.search_result)
					// if (data.code == 0) {
					// 	this.search_result = data.data
					// } else {
					// 	alert("网络异常")
					// }
					
				})
			}
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
