<template>
	<view class="container">
		<view class="header">
			<view class="high-light" v-if="isRandom" @click="toRandom">
				随机笑话
			</view>
			<view class="low-light" v-else @click="toRandom">
				随机笑话
			</view>
			<view class="high-light" v-if="isNew" @click="toNew">
				最新笑话
			</view>
			<view class="low-light" v-else @click="toNew">
				最新笑话
			</view>
			<view class="high-light" v-if="isTime" @click="toTime">
				按时间获取
			</view>
			<view class="low-light" v-else @click="toTime">
				按时间获取
			</view>
		</view>
		<view class="body">
			<view class="randomJoke" v-if="isRandom" v-for="(item, i) in randomJoke">
				<view class="randomJoke-item">{{i + 1}}.{{item.content}}</view>
			</view>
			<view class="randomJoke" v-if="isNew" v-for="(item, i) in newJoke">
				<view class="randomJoke-item">{{i + 1}}.{{item.content}}</view>
			</view>
			<view class="randomJoke" v-if="isTime" v-for="(item, i) in timeJoke">
				<view class="randomJoke-item">{{i + 1}}.{{item.content}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isRandom: true,
				isNew: false,
				isTime: false,
				randomJoke: {},
				newJoke: {},
				timeJoke: {},
				hasJoke: false
			}
		},
		methods: {
			// 十位时间戳
			timeofTen: function (time) {
				return Math.round(new Date(time).getTime()/1000).toString();
			},
			// 显示随机笑话
			toRandom: function () {
				this.isRandom = true;
				this.isNew = false;
				this.isTime = false;
				this.getRandomJoke(1);
			},
			// 显示最新笑话
			toNew: function () {
				this.isRandom = false;
				this.isNew = true;
				this.isTime = false;
				this.getRandomJoke(2)
			},
			// 显示按时间获取的笑话
			toTime: function () {
				this.isRandom = false;
				this.isNew = false;
				this.isTime = true;
				this.getRandomJoke(3)
			},
			// 获取随机笑话
			getRandomJoke: function (type, time = new Date()) {
				let data, url;
				let that = this;
				if (type === 1) {
					data = {
						key: '4ea4b1eefec41019d6c3a9ea5fe03701'
					};
					url = 'http://v.juhe.cn/joke/randJoke.php';
				} else if (type === 2) {
					data = {
						key: '4ea4b1eefec41019d6c3a9ea5fe03701',
						page: 1,
						pagesize: 20
					};
					url = 'http://v.juhe.cn/joke/content/text.php';
				} else if (type === 3) {
					data = {
						sort: 'asc',
						page: 1,
						pagesize: 20,
						time: this.timeofTen(time),
						key: '4ea4b1eefec41019d6c3a9ea5fe03701'
					};
					url = 'http://v.juhe.cn/joke/content/list.php';
				}
				wx.request({
					url,
					method: 'GET',
					data,
					success (res) {
						console.log(res);
						if (res.data.error_code === 0) {
							let result = res.data.result;
							if (type === 1) {
								that.hasJoke = result === [] ? false : true;
								that.randomJoke = result;
							} else if (type === 2) {
								that.hasJoke = result.data === [] ? false : true;
								that.newJoke = result.data;
							} else if (type === 3) {
								that.hasJoke = result.data === [] ? false : true;
								that.timeJoke = result.data;
							}
						}
					},
					fail (res) {
						wx.showToast({
							title: '失败',
							duration: 2000
						})
					}
				})
			}
		},
		onShow () {
			this.toRandom();
		}
	}
</script>

<style lang="scss">
	.container {
		padding: 20px;
		font-size: 14px;
		line-height: 24px;
		.header {
			display: flex;
			width: 100%;
			justify-content: space-around;
			align-items: center;
			text-align: center;
			.high-light {
				background-color: #e3931d;
				color: #f4f0b9;
				font-size: 13px;
				width: 30%;
				margin: 1%;
			}
			.low-light {
				background-color: #cdcdcd;
				color: #8a8a8a;
				font-size: 13px;
				width: 33%;
				margin: 1%;
			}
			
		}
		.body {
			width: 100%;
			.randomJoke {
				background-color: #F0F0F0;
				.randomJoke-item {
					margin: 1%;
					padding: 1%;
					border: 1px solid #ccc;
					background-color: #B2E0DF;
				}
			}
		}
	}
</style>
