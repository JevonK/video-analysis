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
				search_addr_inx: "https://json.heimuer.xyz",
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
