<template>
	
	<view :animation='animationData'>
		
		<view>
			<view class="question_title">
				<view class="question_item_1">
					<text>{{index}}</text><text id="keyword">{{rubbish}}</text>
				</view>
				
				<view class="question_item_1">
					<text>属于哪种垃圾分类</text>
				</view>
				
				<view class="question_item_2">
					<text>(请点击下面的图标选择)</text>
				</view>
			</view>
		</view>

	
	<view>
		<view class="main-classify">
			<view class="" @click="toClassify(0)">
				<image src="../../static/icos/ico-1.jpg" mode="" class="classify-img"></image>
			</view>
			<view class=""  @click="toClassify(1)">
				<image src="../../static/icos/ico-2.jpg" mode="" class="classify-img"></image>
			</view>
		</view>
		
		<view class="main-classify">
			<view class=""  @click="toClassify(2)">
				<image src="../../static/icos/ico-3.jpg" mode="" class="classify-img"></image>
			</view>
			<view class=""  @click="toClassify(3)">
				<image src="../../static/icos/ico-4.jpg" mode="" class="classify-img"></image>
			</view>
		</view>
	</view>
	
	
	<view class="xuhao">
		<view class="crrent">
			{{index}}
		</view>
		
		<view class="gray">
			/10
		</view>
	</view>
	
	
	</view>
</template>

<script>
	
	let questions=[
		{
			id:1,
			name:'干电池',
			type:0,
			classify:'干垃圾'
		},
		{
			id:2,
			name:'西瓜',
			type:1,
			classify:'湿垃圾'
		},
		{
			id:3,
			name:'塑料瓶',
			type:2,
			classify:'可回收物'
		},
		{
			id:4,
			name:'有汞电池',
			type:3,
			classify:'有害垃圾'
		},
		{
			id:5,
			name:'烟头',
			type:0,
			classify:'干垃圾',
		},
		{
			id:6,
			name:'尿不湿',
			type:0,
			classify:'干垃圾',
		},
		{
			id:7,
			name:'废纸',
			type:0,
			classify:'干垃圾',
		},
		{
			id:8,
			name:'一次性杯子',
			type:0,
			classify:'干垃圾',
		},
		
		{
			id:9,
			name:'面包',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:10,
			name:'过期食品',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:11,
			name:'瓜皮果壳',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:12,
			name:'花卉植物',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:13,
			name:'烟头',
			type:0,
			classify:'干垃圾',
		},
		{
			id:14,
			name:'动物内脏',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:15,
			name:'白菜',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:16,
			name:'鸡蛋',
			type:1,
			classify:'湿垃圾',
		}
		
	];
	
	export default {
		data() {
			return {
				index:1,
				scores:0,
				rubbish:'干电池',
				
				animationData:{},
				problems:[]
			}
		},
		onLoad() {
				
				
		},
		onShow() {
			this.index=1;
			//随机生成题目
			getApp().globalData.totalScores=0;
				
			let set =new Set();
			while(set.size<10){
				set.add(Math.round(Math.random()*questions.length));
			}
			
			
			for(let i of set){
				this.problems.push(questions[i]);
			}
			
			this.rubbish = this.problems[this.index-1].name;
		},
		methods: {
			donghua() {
				var animation = uni.createAnimation({
					duration: 5000,
					timingFunction: 'ease',
				})
				this.animation = animation
				animation.opacity(0).translate(-300).step()
				this.animationData = animation.export()
				
				setTimeout(function() {
					animation.translate(10).opacity(0.5).step({
						duration: 0
					})
					this.animationData = animation.export()
				}.bind(this), 400);
				setTimeout(function() {
					animation.translate(0).opacity(1).step({
						duration: 800
					})
					this.animationData = animation.export()
					
				}.bind(this), 500);
				
			},
			toClassify(answer){
				if(this.index>=10){
					
					getApp().globalData.totalScores=this.scores;
					let score='恭喜你'+'，答对了'+this.scores+'道题，加油，再接再厉'
					uni.showModal({
						title:'恭喜你',
						confirmText:'确定',
						content:score,
						showCancel:false,
						success:function(res){
							uni.switchTab({
								url:'../index/index',
							})
						}
					})
					
					
					return;
				}
				
				let that=this;
				let yourAnswer='';
				let rightAnswer=this.rubbish+'属于'+this.problems[this.index-1].classify;
				if(answer===this.problems[this.index-1].type){
					
					this.scores++;
					//下一道题
					this.donghua();
					
				}else{
					if(answer===0){
						yourAnswer='干垃圾'
					}else if(answer===1){
						yourAnswer='湿垃圾'
					}else if(answer===2){
						yourAnswer='有害垃圾'
					}else if(answer===3){
						yourAnswer='可回收物'
					}
					
					uni.showModal({
						title:'您选错了',
						content:rightAnswer,
						confirmText:'知道了',
						showCancel:false,
						
						success:function(res){
							that.donghua();
						}
						
					})
					
				}
				this.index++;
				this.rubbish = this.problems[this.index-1].name;
				
			}
		}
	}
</script>

<style>
	.crrent{
		color: #3F536E;
	}
	
	.gray{
		color: #8F8F94;
	}
	
	#keyword{
		color: #F0AD4E;
	}
	
	.question_title{
		color: #007AFF;
		font-size: 32upx;
		margin-bottom: 15upx;
	}
	
	.question_item_1{
		display: flex;
		justify-content: center;
		align-items: center;
		margin-bottom: 5upx;
	}
	
	.question_item_2{
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 24upx;
		color: #F0AD4E;
		margin-top: 5upx;
	}
	
	.main-classify{
		display: flex;
		justify-content: center;
		direction: center;
	}
	
	.classify-img{
		width: 200upx;
		height: 200upx;
		padding: 20upx;
	}
	
	.xuhao{
		display: flex;
		justify-content: center;
		margin-top: 10upx;
	}
</style>
