<template>
	<view class="pics">
		<scroll-view class="left" scroll-y>
			<view @click="leftClickHandle(index,item.id)" :class="active===index?'active':''"
				v-for="(item,index) in cateList" :key="item.id">
				{{item.title}}
			</view>
		</scroll-view>
		<scroll-view class="right" scroll-y>
			<view class="item" v-for="item in secondData" :key="item.id">
				<image @click="previewImg(item.img_url)"
					src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fc-ssl.duitang.com%2Fuploads%2Fblog%2F202108%2F25%2F20210825124013_2aae8.thumb.1000_0.jpg&refer=http%3A%2F%2Fc-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1679327257&t=ed071e1ff62a428332b39c93f2fd6272">
				</image>
				<text>{{item.title}}</text>
			</view>
			<text v-if="secondData.length === 0">暂无数据</text>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cateList: [],
				active: 0,
				secondData: []
			}
		},
		methods: {
			async getPicsCate() {
				const res = await this.$http({
					url: '/api/getimgcategory'
				})
				this.cateList = res.data.message
				this.leftClickHandle(0, this.cateList[0].id)
			},
			async leftClickHandle(index, id) {
				this.active = index
				// 获取右侧的数据
				const res = await this.$http({
					url: '/api/getimages/' + id
				})
				this.secondData = res.data.message
			},
			previewImg(current) {
				const urls = [
					'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fc-ssl.duitang.com%2Fuploads%2Fblog%2F202108%2F25%2F20210825124013_2aae8.thumb.1000_0.jpg&refer=http%3A%2F%2Fc-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1679327257&t=ed071e1ff62a428332b39c93f2fd6272',
					'https://n.sinaimg.cn/sinacn20111/615/w1135h1080/20190129/d83d-hshmsth6925761.jpg'
				]
				uni.previewImage({
					current,
					urls
				})
			}
		},

		onLoad() {
			this.getPicsCate()
		}
	}
</script>

<style lang="scss">
	page {
		height: 100%;
	}

	.pics {
		height: 100%;
		display: flex;

		.left {
			width: 200rpx;
			height: 100%;
			border-right: 1px solid #eee;

			view {
				height: 60px;
				line-height: 60px;
				color: #333;
				text-align: center;
				font-size: 30rpx;
				border-top: 1px solid #eee;
			}

			.active {
				background: $shop-color;
				color: #fff;
			}
		}

		.right {
			height: 100%;
			width: 520rpx;
			margin: 10rpx auto;

			.item {
				image {
					width: 520rpx;
					height: 520rpx;
					border-radius: 5px;
				}

				text {
					font-size: 30rpx;
					line-height: 60rpx;
				}
			}
		}
	}
</style>
