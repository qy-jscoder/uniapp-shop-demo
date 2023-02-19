<template>
	<view class="content">
		<good-list :goodList="goodList" @goodsItemClick="goodsItemClick" :isReachBottom="isReachBottom"></good-list>
	</view>
</template>

<script>
	import GoodList from '../../components/goodsList/goodsList.vue'
	export default {
		components:{
			GoodList
		},
		data(){
			return {
				pageindex:1,
				goodList:[],
				isReachBottom:false
			}
		},
		methods:{
			async getGoodList(){
				const res=await this.$http({
					url:'/api/getgoods?pageindex='+this.pageindex
				})
				const data=res?.data?.message||[]
				this.goodList = [...this.goodList,...data]
			},
			//下拉刷新重置数据
		    refreshPage(){
				this.pageindex=1
				this.goodList=[]
				this.isReachBottom=false
				setTimeout(async ()=>{
					await this.getGoodList()
					uni.stopPullDownRefresh()
				},1000)
			},
			goodsItemClick(id){
				uni.navigateTo({
					url:'/pages/goodsDetail/goodsDetail?id='+id
				})
			}
		},
		onLoad() {
			this.getGoodList()
		},
		async onReachBottom() {
			if(this.goodList.length%10>0){
				this.isReachBottom=true
			}else{
				this.pageindex++
				await this.getGoodList()
			}
		},
		onPullDownRefresh() {
			this.refreshPage()
		}
	}
</script>

<style lang="scss">
	.content{
		background-color: #eee;
	}
</style>