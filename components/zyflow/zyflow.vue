<template>
	<view  class="zy_content">
		<scroll-view :scroll-with-animation="true" @scroll="scroll_move" lower-threshold="800" @scrolltolower="getdata" scroll-y="true" class="zy_scroll">
			<view class="scr_view">
				<view class="zy_list zy_left">
					<view v-for="(item,index) in flowlist" v-if="index%2==0"  class="zy_card">
						<image @tap="touch_view" :data-index="index" :src="item.src" :key="index" mode="widthFix"></image>
						<view @tap="touch_view" :data-index="index"  class="zy_bottom_view">
							<text class="zy_title">{{item.title}}</text>
							<!--
							<text class="zy_money">¥ {{item.money}}</text>
							<text class="zy_old_money">¥{{item.orig}}</text>
							-->
							<view v-if="item.showText" class="zy_type_view">{{item.text}}</view>
						</view>
					</view>
				</view>
				<view class="zy_list zy_right">
					<view v-for="(item,index) in flowlist" v-if="index%2==1" :key="index" class="zy_card">
						<image @tap="touch_view" :data-index="index"  :src="item.src" mode="widthFix"></image>
						<view @tap="touch_view" :data-index="index"  class="zy_bottom_view">
							<text class="zy_title">{{item.title}}</text>
							<!--
							<text class="zy_money">¥ {{item.money}}</text>
							<text class="zy_old_money">¥{{item.orig}}</text>
							-->
							<view v-if="item.showText" class="zy_type_view">{{item.text}}</view>
						</view>
					</view>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		props:{
			flowlist:{
				type:Array,
				default: []
			}
		},
		data() {
			return {
		
				
			};
		},
		methods:{
			//卡片点击事件
			touch_view(e){
				this.$emit('flowtap',this.flowlist[e.currentTarget.dataset.index]);
			},
			//滑到底部获取数据
			getdata(){
				this.$emit('flowget');
			},
			//滚动事件
			scroll_move(e){
				
				if(e.detail.scrollTop>1000){
					if(this.show_flexd==false){
						this.show_flexd=true;
					}
				}else{
					if(this.show_flexd==true){
						this.show_flexd=false;
					}
				}
			}
		}
	}
</script>

<style>
	.zy_content{
		width: 100%;
		height: 100%;
		background-color: #f6f6f6;
		position: relative;
	}
	.zy_scroll{
		width: 100%;
		height: 100%;
	}
	.scr_view{
		width: 100%;
		height: 100%;
		display: flex;
		justify-content: space-evenly;
	}
	.zy_list{
		width: 45%;
		/* border: 1px solid red; */
	}
	.zy_left{
		/* border: 1px solid #000; */
	}
	.zy_right{
		/* border: 1px solid red; */
	}
	.zy_card{
		width: 100%;
		background-color: #fefefe;
		border-radius: 10rpx;
		overflow: hidden;
		box-shadow:1px 1px 5px gray;
		margin-top: 20rpx;
	}
	.zy_card image{
		width: 100%;
		margin-bottom:5rpx;
	}
	/* 底部卡片内容 */
	.zy_bottom_view{
		width: 100%;
		min-height: 100rpx;
		background-color: #fff;
		margin-bottom: 15rpx;
	}
	.zy_title{
		display: inline-block;
		width: 90%;
		max-height: 70rpx;
		margin-left: 5%;
		margin-top: 10rpx;
		font-size: 26rpx;
		font-weight: bold;
		color: #252525;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
	}
	.zy_money{
		display:block;
		width: 90%;
		margin-left: 5%;
		margin-top: 5rpx;
		color: #f35d3c;
		font-size: 32rpx;
		font-weight: bold;
	}
	.zy_old_money{
		display:block;
		width: 90%;
		margin-left: 5%;
		color: #acacac;
		font-size: 20rpx;
		text-decoration: line-through;
		margin-top: 5rpx;
	}
	.zy_type_view{
		display: inline-block;
		background-color: #fff3f3;
		margin-bottom: 10rpx;
		color: #e36161;
		font-size: 25rpx;
		padding: 2rpx 5px;
		border-radius:0 5rpx;
		margin-left: 5%;
	}
	
</style>
