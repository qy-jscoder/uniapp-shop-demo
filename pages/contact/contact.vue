<template>
	<view class="contact">
		<image class="img" src="http://www.itcast.cn/2018czydz/images/gywmban.jpg"></image>
		<view class="info">
			<view @click="phone">联系电话：2112345 ( 点击拨打 )</view>
			<view>地址：南京市玄武区北京东路41号</view>
		</view>
		<map class="map" :longitude="longitude" :scale="scale" :latitude="latitude" :markers="markers"></map>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				longitude: 118.803129,
				latitude: 32.065761,
				scale: 13,
				markers: [{//地点标记
					longitude: 118.803129,
					latitude: 32.065761,
					iconPath: '../../static/logo.png',
					width: 30,
					height: 30
				}]
			}
		},
		methods: {
			phone() {
				uni.makePhoneCall({
					phoneNumber: '2112345'
				})
			}
		}
	}
</script>

<style lang="scss">
	.contact {
		.img {
			width: 750rpx;
			height: 320rpx;
		}

		.info {
			padding: 10rpx 20rpx;
			font-size: 30rpx;

			view {
				line-height: 80rpx;
				border-bottom: 1px solid #eee;
			}
		}

		.map {
			width: 750rpx;
			height: 750rpx;
		}
	}
</style>
