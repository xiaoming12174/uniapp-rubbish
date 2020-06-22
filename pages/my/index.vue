<template>
	<view class="center">
		<view class="center_top">
			<view class="mask"></view>
		</view>
		<view class="center_box_bg">
			<view class="profily">
				<view class="base">
					
					<image :src="avatarUrl" mode="" id="avatarUrl"></image>
				
					<text>{{nickname}}</text>
					<button v-if="nickname==='昵称'" type="default" open-type="getUserInfo" @getuserinfo="getUserInfo" id="getUserInfo">使用微信登录</button>
					<image src="../../static/fumou-center-template/setting.png" mode=""></image>
				</view>
				<view class="order_status">
					<view class="status" v-for="item in status" :key='item.key'>
						<image class="icon" :src="item.url" mode="aspectFill"></image>
						<text>{{item.name}}</text>
					</view>
				</view>
			</view>
			<view class="baiban">

			</view>
			<view class="center_menu">
				<view class="menu_item" v-for="item in menus" :key='item.key'>
					<image :src="item.icon" mode="aspectFill"></image>
					<text>{{item.name}}</text>
					<text v-if="scores!=0">{{scores}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				scores:0,
				avatarUrl:'../../static/fumou-center-template/header.jpg',
				nickname:'昵称',
				status: [{
						key: 1,
						name: '待发货',
						url: '../../static/fumou-center-template/one.png'
					},
					{
						key: 2,
						name: '待收货',
						url: '../../static/fumou-center-template/2.png'
					},
					{
						key: 3,
						name: '待评价',
						url: '../../static/fumou-center-template/3.png'
					},
					{
						key: 4,
						name: '全部订单',
						url: '../../static/fumou-center-template/4.png'
					}
				],
				menus: [{
						name: '我的收藏',
						icon: '../../static/fumou-center-template/5.png',
						key: 1,
					},
					{
						name: '地址管理',
						icon: '../../static/fumou-center-template/6.png',
						key: 2,
					},
					{
						name: '尺码对照表',
						icon: '../../static/fumou-center-template/7.png',
						key: 3,
					},
					{
						name: '帮助中心',
						icon: '../../static/fumou-center-template/8.png',
						key: 4,
					},
					{
						name: '意见反馈',
						icon: '../../static/fumou-center-template/9.png',
						key: 5,
					},
					{
						name: '关于我们',
						icon: '../../static/fumou-center-template/10.png',
						key: 6,
					}

				]
			};
		},
		methods: {
				getUserInfo(){
					let that=this;
					
					uni.login({
						provider:'weixin',
						success:function(loginRes){
							console.log(loginRes.authResult);
							
							uni.getUserInfo({
								provider:'weixin',
								success:function(infoRes){
									console.log(infoRes)
									console.log('用户昵称为'+infoRes.userInfo.nickName);
									
									that.avatarUrl=infoRes.userInfo.avatarUrl;
									that.nickname=infoRes.userInfo.nickName;
								}
							});
						}
					});
				}
		},
		computed: {

		},
		onLoad() {
			
		}
	}
</script>

<style lang="scss">
	page {
		height: 100%;
	}

	#avatarUrl{
		position: relative;
		height: 120upx;
		width: 120upx;
	}
	#getUserInfo{
		border-radius: 45upx;
		background-color: #F1F1F1;
		font-size: 34upx;
	}
	
	.profily,
	.profily_header {
		border-radius: 8px;
	}

	.center {
		height: 100%;

		&_top {
			height: 18%;
			background: url('../../static/fumou-center-template/header.jpg') no-repeat 0% 50%;
			background-size: cover;

			// background: #E6E6E6;
			.mask {
				background: rgba(0, 0, 0, .4);
				height: 100%;
			}
		}

		&_box_bg {
			background: #F9F9F9;
			position: relative;

			.profily {
				position: absolute;
				width: 90%;
				// border:1px solid #F7F7F7;
				margin: 0 auto;
				top: -100upx;
				left: 5%;
				background: #FEFEFE;
				padding: 35upx;
				box-sizing: border-box;
				box-shadow: 0px 2px 5px #EDEDED;
			}
		}
	}

	.base {
		display: flex;
		align-items: center;
		border-bottom: 2px solid #F6F6F6;
		padding-bottom: 35upx;
		position: relative;
		.profily_header {
			height: 120upx;
			width: 120upx;
			background-image: url('../../static/fumou-center-template/header.jpg');
			background-size: 100%;
		}

		text {
			margin-left: 20px;
			font-size: 30upx;
		}
		
		image{
			position: absolute;
			height: 40upx;
			width: 40upx;
			right: 0px;
			top:0px;
		}
	}

	.order_status {
		display: flex;
		justify-content: space-between;
		margin-top: 35upx;

		.status {
			width: 140upx;
			font-size: 24upx;
			text-align: center;
			letter-spacing: .5px;
			display: flex;
			flex-direction: column;
			.icon {
				width: 50upx;
				height: 50upx;
				margin: 0 auto;
				margin-bottom: 5px;
				
			}
		}
	}

	.baiban {
		background: #FEFEFE;
		height: 300upx;
	}

	.center_menu {
		width: 100%;
		display: inline-block;

		.menu_item {
			font-size: 28upx;
			letter-spacing: 1px;
			padding: 14px 5%;
			background: #FEFEFE;
			overflow: hidden;
			box-sizing: border-box;
			display: flex;
			align-items: center;
			position: relative;
			border-bottom: 1px solid #EFEFEF;

			&:hover {
				background: #F6F6F6 !important;
			}

			&::after {
				content: '';
				width: 30upx;
				height: 30upx;
				position: absolute;
				right: 5%;
				background: url('../../static/fumou-center-template/right.png') no-repeat;
				background-size: 100%;
			}

			text:nth-of-type(1) {
				margin-left: 10px;
			}

			image {
				width: 40upx;
				height: 40upx;
			}

			&:nth-of-type(4) {
				margin-top: 10px;
			}
		}
	}
</style>

