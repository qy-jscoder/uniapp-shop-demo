<template>
	<view>
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" circular>
			<swiper-item v-for="item in imgList" :key="item.id">
				<image :src="item.img"></image>
			</swiper-item>
		</swiper>
		<view class="nav">
			<view class="nav-item" v-for="item in navList" :key="item.icon" @click="navLink(item.url)">
				<view :class="item.icon" class="iconfont"></view>
				<text>{{item.name}}</text>
			</view>
		</view>
		<view class="recommend">
			<view class="tip">推荐商品</view>
			<good-list :goodList="goodList" :isReachBottom="isReachBottom" @goodsItemClick="goodsItemClick"></good-list>
		</view>
	</view>
</template>

<script>
	import GoodList from '../../components/goodsList/goodsList.vue'
	export default {
		data() {
			return {
				imgList: [],
				goodList: [],
				pageindex:1,
				isReachBottom:false,
				navList: [{
						icon: 'icon-ziyuan',
						name: 'xx商品',
						url:'/pages/goods/goods'
					},
					{
						icon: 'icon-guanyuwomen',
						name: '联系我们',
						url:'/pages/contact/contact'
					},
					{
						icon: 'icon-tupian',
						name: '社区图片',
						url:'/pages/pics/pics'
					},
					{
						icon: 'icon-shipin',
						name: '学习视频'
					}
				]
			}
		},
		onLoad() {
			this.getLunbo()
			this.getHotGoods()
		},
		methods: {
			//获取轮播图的数据
			async getLunbo() {
				const res = await this.$http({
					url: '/api/getLunbo'
				})
				this.imgList = res?.data?.message || []
			},
			// 获取热门商品列表数据
			async getHotGoods() {
				const res = await this.$http({
					url: '/api/getgoods?pageindex='+this.pageindex
				})
				const data=res?.data?.message||[]
				this.goodList = [...this.goodList,...data]
			},
			navLink(url){
				uni.navigateTo({
					url
				})
			},
			goodsItemClick(id){
				uni.navigateTo({
					url:'/pages/goodsDetail/goodsDetail?id='+id
				})
			}
		},
		components:{
			GoodList
		},
		async onReachBottom() {
			if(this.goodList.length%10>0){
				this.isReachBottom=true
			}else{
				this.pageindex++
				await this.getHotGoods()
			}
		}
	}
</script>

<style lang="scss">
	swiper {
		width: 750rpx;
		height: 380rpx;

		image {
			width: 100%;
			height: 100%;
		}
	}

	.nav {
		display: flex;
		justify-content: space-between;

		.nav-item {
			flex: 1;
			text-align: center;

			view {
				width: 120rpx;
				height: 120rpx;
				background-color: #b50e03;
				border-radius: 50%;
				text-align: center;
				margin: 10px auto;
				line-height: 120rpx;
				color: #fff;
				font-size: 50rpx;
			}

			text {
				font-size: 30rpx;
			}
		}
	}

	.recommend {
		background-color: #eee;
		overflow: hidden;
		margin-top: 10px;

		.tip {
			height: 50px;
			line-height: 50px;
			color: $shop-color;
			text-align: center;
			background-color: #fff;
			letter-spacing: 20px;
			margin: 5px auto;
		}

		
	}
</style>
