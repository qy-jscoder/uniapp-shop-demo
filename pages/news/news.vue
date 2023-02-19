<template>
	<view class="news">
		<news-item @itemClick="goDetail" :list="newsList"></news-item>
	</view>
</template>

<script>
	import NewsItem from '../../components/newsItem/newsItem.vue'
	export default {
		data() {
			return {
				newsList: []
			}
		},
		methods: {
			async getNews() {
				const res = await this.$http({
					url: '/api/getnewslist'
				})
				this.newsList = res.data.message
			},
			goDetail(id) {
				console.log(id,'xxx');	
				uni.navigateTo({
					url: '/pages/newsDetail/newsDetail?id=' + id
				})
			}
		},
		components: {
			NewsItem
		},
		onLoad() {
			this.getNews()
		}
	}
</script>

<style lang="scss">
	.news {}
</style>
