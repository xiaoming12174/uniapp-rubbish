<template>
	<view >
		<!--轮播图-->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" class="swiper-flex">
			<swiper-item v-for="(item,index) in swipers" :key="index">
				<view class="swiper-item">
					<image :src="item" mode="" class="swiper-img"></image>
				</view>
			</swiper-item>
		</swiper>
		<!--搜索框-->
		<view >
			<!-- 提示框-->
			<view class="tip-text">
				你知道[干电池]是什么垃圾吗
			</view>
			<form @submit="formSubmit" @reset="formReset">
				<view class="input-view">
					
					<view class="input-view-item" @click="imgShiBie">
						<image src="../../static/icos/camera.png" mode="" class="search-img"></image>
					</view>
					<!--语音-->
					<view class="input-view-item" @touchstart="startRecord" @touchend="endRecord">
						<image src="../../static/icos/record.png" mode="" class="search-img"></image>
					</view>
					
					<view class="input-view-item">
						<input type="text" v-model.trim="keyword" placeholder="请输入垃圾关键词"  class="search-text"/>
					</view>
					
					<view class="font-search" @tap="searchKeyword">
						查询
					</view>
				</view>
			</form>
			
		</view>
		
		<view>
			<view class="simepleTips">
				{{tip}}
			</view>
			
			
		</view>
		<!--大分类-->
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
				<view class=""  @click="toClassify(3)">
					<image src="../../static/icos/ico-3.jpg" mode="" class="classify-img"></image>
				</view>
				<view class=""  @click="toClassify(2)">
					<image src="../../static/icos/ico-4.jpg" mode="" class="classify-img"></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	const recorderManager = uni.getRecorderManager();
	const innerAudioContext = uni.createInnerAudioContext();
	
	innerAudioContext.autoplay = true;
	
	export default {
		data() {
			return {
				 voicePath: '',
				swipers:[
					'../../static/swipers/lunbo1.jpg',
					'../../static/swipers/lunbo2.jpg',
					'../../static/swipers/lunbo3.jpg'
				],
				keyword:'塑料袋',
				searchList:[],
				tip:'',
				simpleTips:[
					          "让垃圾找到自己的归属",
					          "请给垃圾找个合适的家",
					          "希望有一天，垃圾桶也会下岗",
					          "垃圾分类放，环境有保障",
					          "垃圾要分类，资源要利用",
					          "今天分一分，明天美十分",
					          "积极参与垃圾分类，共创优美社区环境",
					          "分类收集人人有责，男女老幼齐来参与",
					          "提高社区的品味，从垃圾分类开始",
					          "垃圾分类，举手之劳",
					          "配合垃圾分类，争做文明市民",
					          "垃圾分类益处多，环境保护靠你我",
					          "要是垃圾变为宝，分类回收不可少",
					          "请给垃圾找个合适的家",
					          "垃圾儿女要分家，安居乐业靠大家",
					          "垃圾分类人人做，做好分类为人人",
					          "让垃圾找到自己的归属",
					          "垃圾要分类，生活变美好",
					          "你我虽渺小，但意义重大",
					          "让垃圾找到自己的归属",
					          "请给垃圾找个合适的家",
				]
			}
		},
		//生命周期
		onShow() {
			let index=Math.round(Math.random() * this.simpleTips.length);
			this.tip=this.simpleTips[index];
			
			

		},
		
	  onLoad() {
			let self = this;
			recorderManager.onStop(function (res) {
				console.log('recorder stop' + JSON.stringify(res));
				self.voicePath = res.tempFilePath;
				let src=res.tempFilePath;
				
				
				//拿到数据之后，发送http请求
				uni.uploadFile({
				  url: 'http://api.tianapi.com' + '/txapi/voicelajifenlei/index', //语音垃圾分类接口
				  method:'POST',
				  header:{
				  	'content-type':'multipart/form-data'
				  },
				  filePath: src,
				  name: 'say',
				  formData: {
				    key: 'bd14dcaf2d549a06a60164f6cb58b652',
				    format:'pcm'
				  },
				  success (res){
				    console.log(res.data)
				    wx.showModal({
				      title: '提示',
				      content: res.data,
				      showCancel: false
				    })
				  
				  }
				})
				
				
			});
		},
		
		methods: {
			//图形识别
			imgShiBie(){
				wx.chooseImage({
					count:1,
					sourceType:['album','camera'],
					sizeType:['compressed'],
					success:function(result){
						wx.getFileSystemManager().readFile({
							filePath:result.tempFilePaths[0],
							encoding:'base64',
							
							success:res=>{
								console.log('base64'+res.data);
								
								wx.request({
									url:'http://api.tianapi.com/txapi/imglajifenlei/index',
									method:'POST',
									header:{
										'content-type':'application/x-www-form-urlencoded'
									},
									data:{
										key:'bd14dcaf2d549a06a60164f6cb58b652',
										img:'data:image/png;base64,'+res.data,
										mode:1
									},
									success:function(response){
										console.log(response);
										
										getApp().globalData.rubbishList=[];
										for (let rubbish of response.data.newslist) {
											let obj={};
											
											if(rubbish.lajitype==1){
												obj.src='../../static/icos/ico-4.jpg';
											}else if(rubbish.lajitype==3){
												obj.src='../../static/icos/ico-1.jpg';
											}else if(rubbish.lajitype==2){
												obj.src='../../static/icos/ico-2.jpg';
											}else if(rubbish.lajitype==0){
												obj.src='../../static/icos/ico-3.jpg';
											}else if(rubbish.lajitype==4){
												obj.src='../../static/icos/ico-5.jpg';
											}
											
											obj.title=rubbish.lajitype;
											obj.showText=true;
											obj.text=rubbish.name;
											getApp().globalData.rubbishList.push(obj);
											
											uni.navigateTo({
												url:'../detail/index'
											})
										}
									}
								});
							}
							
						});
						
					}
				});
				
				
			},
			
			//关键字查询
			searchKeyword(){
				this.request({
					url:'http://api.tianapi.com/txapi/lajifenlei/index',
					data:{
						key:'bd14dcaf2d549a06a60164f6cb58b652',
						word:this.keyword
					}
					
				}).
				then(response=>{
					console.log(response);
					getApp().globalData.rubbishList=[];
					for (let rubbish of response.newslist) {
						let obj={};
						
						if(rubbish.type==1){
							obj.src='../../static/icos/ico-4.jpg';
						}else if(rubbish.type==3){
							obj.src='../../static/icos/ico-1.jpg';
						}else if(rubbish.type==2){
							obj.src='../../static/icos/ico-2.jpg';
						}else if(rubbish.type==0){
							obj.src='../../static/icos/ico-3.jpg';
						}
						
						obj.title=rubbish.explain;
						obj.showText=true;
						obj.text=rubbish.name;
						getApp().globalData.rubbishList.push(obj);
						
						uni.navigateTo({
							url:'../detail/index'
						})
					}
					
				}
				)
			},
			
			toClassify(current){
				getApp().globalData.current=current;
				
				
				uni.switchTab({
					url:'/pages/type/index'
				});
			},
			
			//语音部分
			 startRecord() {
			            console.log('开始录音');
			
			            recorderManager.start(
						{
							duration: 20000,
							numberOfChannels:2,
							sampleRate:16000,

							format: 'aac'
						}
						);
			},
			endRecord() {
				console.log('录音结束');
				recorderManager.stop();
			}
		}
	}
</script>

<style>
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
	
	.simepleTips{
		display: flex;
		justify-content: center;
		font-size: 36upx;
		color:#39a13e;
	}
	
	.swiper-img{
		width: 750upx;
		height: 300upx;
	}
	
	.swiper-flex{
		display: flex;
		justify-content: center;
	}
	
	.tip-text{
		text-align: center;
		margin-top: 5rpx;
		font-size: 24rpx;
	}
	
	.input-view{
		display: flex;
		direction: row;
		justify-content: center;
		align-items: center;
		border: 1rpx solid #00f375;
		border-radius: 50upx;
		box-shadow: 10upx 10upx 20upx #FFFFFF;
		margin: 10upx 20upx 20upx 20upx 20upx;
	}
	
	.search-img{
		width: 80upx;
		height: 80upx;
	}
	
	.input-view-item{
		height: 80upx;
		align-items: center;
		padding: 8upx;
	}
	
	.search-text{
		height: 80upx;
		text-align: center;
		width: 350upx;
	}
	
	.font-search{
		height: 80upx;
		padding: 10upx;
		color: #4CD964;
		font-weight: bold;
		margin-top: 30upx;
	}
</style>
