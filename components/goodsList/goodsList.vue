<template>
	<view class="">
		<view class="good-list">
			<view class="good-item" v-for="item in goodList" :key="item.id" @click="navigator(item.id)">
				<image src="/static/logo.png" alt="图片"></image>
				<view class="price">
					<text>{{item.sell_price}}</text>
					<text>{{item.market_price}}</text>
				</view>
				<view class="name">{{item.title}}</view>
			</view>
		</view>
		<view v-if="isReachBottom" class="bottom-tip">到底了</view>
	</view>
</template>

<script>
	export default {
		props: {
			goodList: {
				type: Array,
				default: () => []
			},
			isReachBottom:{
				type:Boolean,
				default:false
			}
		},
		methods: {
			navigator(id) {
				this.$emit('goodsItemClick', id)
			}
		}
	}
</script>

<style lang="scss">
	.good-list {
		padding: 0 15rpx;
		display: flex;
		justify-content: space-between;
		flex-wrap: wrap;

		.good-item {
			background-color: #fff;
			width: 355rpx;
			margin: 10rpx 0;
			padding: 15rpx;
			box-sizing: border-box;

			image {
				width: 80%;
				height: 150px;
				display: block;
				margin: auto;
			}

			.price {
				color: $shop-color;
				font-size: 36rpx;

				text:nth-child(2) {
					color: #ccc;
					font-size: 28rpx;
					margin-left: 5px;
					text-decoration: line-through;
				}
			}

			.name {
				font-size: 28rpx;
				line-height: 50rpx;
				padding-bottom: 15rpx;
				padding-top: 10rpx;
			}
		}
	}
	.bottom-tip{
		text-align: center;
		color: #ccc;
	}
</style>
