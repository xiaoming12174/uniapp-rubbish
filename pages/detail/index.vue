<template>
	<view class="content">
		<zyflow :flowlist="datalist" @flowtap="touchcard" @flowget="adddata" ></zyflow>
	</view>
</template>

<script>
	// 导入瀑布流组件
	import zyflow from "../../components/zyflow/zyflow.vue";
	export default {
		//注册组件
		components:{
			zyflow
		},
		data() {
			return {
				datalist:[
					
				]
			}
		},
		onShow() {
			this.datalist=[];
			this.datalist=getApp().globalData.rubbishList;
		},
		methods: {
			touchcard(e){
				console.log('点击此项获取到的数据')
				console.log(e);	
			},
			adddata(){
			
			}
		}
	}
</script>

<style>
	.content {
		width: 100%;
		height: 100%;
		position: absolute;
		background-color: #bfa;
	}
</style>

