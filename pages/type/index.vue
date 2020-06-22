<template>
	<view>
		<view class="tab">
			 <uni-segmented-control 
			 :current="current" 
			 :values="items.map(v=>v.title)" 
			 @clickItem="onClickItem" 
			 style-type="button" 
			 active-color="#4cd964">
			 </uni-segmented-control>
		</view>
		
		<view>
			<view v-if="current==0">
				<GanRubbish></GanRubbish>
			</view>
			<view v-if="current==1">
				<ShiRubbish></ShiRubbish>
			</view>
			<view v-if="current==2">
				<YouHaiRubbish></YouHaiRubbish>
			</view>
			<view v-if="current==3">
				<KeHuiShouRubbish></KeHuiShouRubbish>
			</view>
		</view>
		
	</view>
	
</template>

<script>
	import GanRubbish from './rubbish-gan/index.vue'
	import ShiRubbish from './rubbish-shi/index.vue'
	import YouHaiRubbish from './rubbish-yhlj/index.vue'
	import KeHuiShouRubbish from './rubbish-khs/index.vue'
	
	import {uniSegmentedControl} from '@dcloudio/uni-ui'
	export default {
		data() {
			return {
				items:[
					{title:'干垃圾'},
					{title:'湿垃圾'},
					{title:'有害垃圾'},
					{title:'可回收物'}
				],
				current:0
			}
		},
		onShow() {
			this.current=getApp().globalData.current;
		},
		methods: {
			onClickItem(e){
				if(this.current !== e.currentIndex){
					this.current=e.currentIndex;
				}
			}
		},
		components:{
			GanRubbish,
			ShiRubbish,
			YouHaiRubbish,
			KeHuiShouRubbish,
			uniSegmentedControl
		}
	}
</script>

<style>
	
</style>
