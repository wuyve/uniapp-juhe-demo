<template>
	<view class="content">
		<view class="header">
			<view class="upper">
				<image src="../../static/happy/back.jpg" mode="scaleToFill" class="upper-image"></image>
				<view class="upper-text">老黄历</view>
			</view>
			<view class="down">
				<view class="left">
					<image src="../../static/happy/back1.jpg" mode="scaleToFill" class="left-image"></image>
					<view class="left-text">星座运势</view>
				</view>
				<view class="right">
					<image src="../../static/happy/back2.jpg" mode="scaleToFill" class="right-image"></image>
					<view class="right-text">生肖配对</view>
				</view>
			</view>
		</view>
		<view class="main-body">
			<view class="main-body-title">周公解梦</view>
			<view class="search">
				<view class="search-type">
					<view class="search-type-text">分类</view>
					<picker mode="selector" :range="searchType" :value="searchType[index].fid" :range-key="'name'" @change="selectType">
						<view class="search-type-item">{{searchType[index].name}}</view>
					</picker>
				</view>
				<view class="search-keyword">
					<view class="search-keyword-text">关键词</view>
					<input type="text" v-model="searchText" placeholder="请输入关键词" class="search-keyword-input" />
				</view>
				<view class="search-btn" @click="search">
					搜索
				</view>
			</view>
			<view class="desc" v-if="showDesc">
				<view class="desc-img">
					<image src="../../static/happy/dream.png"></image>
				</view>
				<view class="desc-text">
					<view class="desc-text-header">
						<view class="desc-name">周公</view>
						<view class="desc-oldname">姓姬名旦</view>
					</view>
					<view class="desc-text-content">每个人都要做梦，梦与之俱来，随之而去，伴随人之一生。梦如同人的身影一般，既司空见惯，又神秘莫测，既虚无缥缈，又真实可见。若说梦是幻觉，然梦中之人物事件，醒后皆历历在目；若说梦是真实之表现，然醒后难找与梦中人物事件完全一致者。</view>
				</view>
			</view>
			<view v-if="showDesc" style="display: flex; align-items: center; justify-content: center; margin-top: 30px;">什么也没有，来试试玩吧~~</view>
			<view class="result" v-if="!showDesc" v-for="(item, i) in result">
				<view class="result-keyword">
					<view class="result-keyword-text">关键词</view>
					<view class="result-keyword-word">{{item.title}}</view>
				</view>
				<view class="result-desc">
					<view class="result-desc-text">描述</view>
					<view class="result-desc-word">{{item.des}}</view>
				</view>
				<view class="result-content">
					<view class="result-content-text">更多</view>
					<view class="result-content-word">
						<view class="more-word" v-for="(sub, j) in item.list">
							<view class="more-word-item">{{sub}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				searchType: [],
				index: 0,
				searchText: '',
				showDesc: true,
				result: []
			}
		},
		methods: {
			selectType: function (e) {
				console.log('这个type是啥子呢？？？', e);
				this.index = e.detail.value;
			},
			// 获取周公解梦的类别
			getDreamType: function () {
				let data = {
					key: '622038d1325de57325038a24f066093c'
				};
				let that = this;
				wx.request({
					url: 'http://v.juhe.cn/dream/category?key=622038d1325de57325038a24f066093c',
					method: 'GET',
					data,
					success (res) {
						let obj = {
							name: '全部',
							fid: ''
						};
						that.searchType = res.data.result;
						console.log(that.searchType);
						that.searchType.unshift(obj);
					}
				})
			},
			search: function () {
				let data = {
					key: '622038d1325de57325038a24f066093c',
					q: this.searchText,  // 关键字
					cid: this.searchType[this.index].fid,  // 分类
					full: 1
				};
				let that = this;
				this.showDesc = false;
				wx.request({
					url: 'http://v.juhe.cn/dream/query',
					method: 'GET',
					data,
					success (res) {
						console.log(res.data);
						that.result = res.data.result;
					}
				})
				console.log(data);
			}
		},
		onLoad: function () {
			this.getDreamType();
		}
	}
</script>

<style lang="scss">
	.content {
		font-size: 15px;
		.header {
			margin: 10px;
			.upper {
				position: relative;
				top: 1%;
				left: 4%;
				.upper-image {
					height: 70px;
				}
				.upper-text {
					position: absolute;
					top: 26%;
					color: #4C4578;
					font-size: 20px;
					font-weight: bold;
					letter-spacing: 20px;
					left: 30%;
				}
			}
			.down {
				margin: 7px;
				position: relative;
				.left {
					width: 50%;
					float: left;
					.left-image {
						width: 100%;
						height: 50px;
					}
					.left-text {
						position: absolute;
						top: 15px;
						color: #FFF;
						left: 14%;
						font-weight: blod;
					}
				}
				.right {
					width: 50%;
					float: right;
					.right-image {
						width: 100%;
						height: 50px;
					}
					.right-text {
						position: absolute;
						top: 15px;
						right: 15%;
						color: #fff;
						font-weight: blod;
					}
				}
			}
		}
		.main-body {
			margin-top: 65px;
			margin-left: 5px;
			margin-right: 10px;
			.main-body-title {
				margin-top: 10px;
				background-color: #07C160;
				width: 80px;
				height: 26px;
				border-radius: 6px;
				display: flex;
				align-items: center;
				justify-content: center;
				color: #FFF;
			}
			.search {
				display: flex;
				align-items: center;
				justify-content: space-between;
				margin-top: 10px;
				.search-type {
					display: flex;
					align-items: center;
					.search-type-item {
						border: 1px solid #000;
						width: 65px;
						display: flex;
						align-items: center;
						justify-content: center;
						color: #11437D;
					}
				}
				.search-keyword {
					display: flex;
					align-items: center;
					.search-keyword-input {
						width: 140px;
						border: 1px solid #000;
						margin-left: 5px;
					}
				}
				.search-btn {
					background-color: #0C3E76;
					color: #fff;
					width: 40px;
					height: 25px;
					border-radius: 4px;
					display: flex;
					align-self: center;
					justify-content: center;
				}
			}
			.desc {
				margin-top: 10px;
				display: flex;
				align-items: center;
				justify-content: flex-start;
				.desc-img {
					width: 105px;
					image {
						width: 105px;
						height: 130px;
					}
				}
				.desc-text {
					font-size: 12px;
					margin-left: 10px;
					margin-right: 10px;
					.desc-text-header {
						display: flex;
						justify-content: space-between;
						.desc-name {
							color: #3388FF;
						}
						.desc-oldname {
							color: #F76603;
						}
					}
					.desc-text-content {
						margin-top: 5px;
						border-top: 1px solid #233437;
					}
				}
			}
			.result {
				border: 1px dotted #ccc;
				margin-left: 5px;
				padding: 5px;
				border-radius: 10px;
				margin-top: 10px;
				margin-right: 5px;
				font-size: 13px;
				.result-keyword {
					display: flex;
					.result-keyword-text {
						width: 50px;
					}
					.result-keyword-word {
						margin-left: 5px;
						color: #3390FF;
					}
				}
				.result-desc {
					display: flex;
					margin-top: 5px;
					margin-bottom: 5px;
					.result-desc-text {
						width: 50px;
					}
					.result-desc-word {
						margin-left: 5px;
						color: #FF7C6D;
					}
				}
				.result-content {
					display: flex;
					align-items: center;
					.more-word {
						margin-bottom: 2px;
						border-bottom: 1px solid #ccc;
						color: #3F3F3F;
					}
				}
			}
		}
	}
</style>
