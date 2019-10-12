<template>
	<view class="page">
		<!-- 使用自定义标签 -->
<!-- 		<helloComp myval="hello next~~"></helloComp>
		<trailerStars innerScore="9.1" showNum = "1"></trailerStars> -->
		<!-- 轮播图start -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" class="carousel">
<!-- 			<swiper-item>
				<image src="../../static/carousel/batmanvssuperman.png" class="carousel"></image>
			</swiper-item>
			<swiper-item>
				<image src="../../static/carousel/spiderman.png" class="carousel"></image>
			</swiper-item> -->
			
			<swiper-item v-for="carousel in carouselList">
				<image :src="carousel.image" class="carousel"></image>
			</swiper-item>			
		</swiper>
		<!-- 轮播图 end-->
		
		<!-- 热门超英 start -->
		<view class="page-block super-hot">
			<view class="hot-tittle-wapper">
				<image src="../../static/icos/hot.png" class="hot-ico"></image>
				<view class="hot-title">
					热门超英
				</view>
			</view>
		</view>
		
		<scroll-view scroll-x="true" class="page-block hot">
			<view class="single-poster" v-for="superhero in hotSuperheroList">
				<view class="poster-wapper">
					<image :src="superhero.cover" class="poster"></image>
					<view class="movie-name">
						{{superhero.name}}
					</view>
					<trailerStars :innerScore="superhero.score" showNum = "1"></trailerStars>
<!-- 					<view class="movie-score-wapper">
						<image src="../../static/icos/star-yellow.png" class="star-ico"></image>
						<image src="../../static/icos/star-yellow.png" class="star-ico"></image>
						<image src="../../static/icos/star-yellow.png" class="star-ico"></image>
						<image src="../../static/icos/star-yellow.png" class="star-ico"></image>
						<image src="../../static/icos/star-yellow.png" class="star-ico"></image>
						<image src="../../static/icos/star-yellow.png" class="star-ico"></image>
						<view class="move-score">
							9.1
						</view>
					</view> -->
				</view>
			</view>
		</scroll-view>
		<!-- 热门超英 end -->
		
		<!-- 热门预告 start -->
		<view class="page-block super-hot">
			<view class="hot-tittle-wapper">
				<image src="../../static/icos/interest.png" class="hot-ico"></image>
				<view class="hot-title">
					热门预告
				</view>
			</view>
			
			<view class="hot-movies page-block">
				<video v-for="trailer in hotTrailerList" :src="trailer.trailer" :poster="trailer.poster" class="hot-move-single"></video>
			</view>
		</view>
		<!-- 热门预告 end -->
		
		<!-- 猜你喜欢 start -->
		<view class="page-block super-hot">
			<view class="hot-tittle-wapper">
				<image src="../../static/icos/guess-u-like.png" class="hot-ico"></image>
				<view class="hot-title">
					猜你喜欢
				</view>
			</view>
			
			<view class="page-block guess-u-like">
				<view class="single-like-movie">
					<image src="../../static/carousel/batmanvssuperman.png" class="like-movie"></image>
					
					<view class="movie-desc">
						<view class="movie-title">
							蝙蝠侠大战超人
						</view>
						<!-- <trailerStars :innerScore="superhero.score" showNum = "0"></trailerStars> -->
						<trailerStars :innerScore="9.1" showNum = "0"></trailerStars>
						<view class="movie-info">
							2018/美国
						</view>
						<view class="movie-info">
							周杰伦
						</view>	
					</view>
					<view class="movie-oper" @click="praiseMe">
						<image src="../../static/icos/praise.png" class="praise-ico"></image>
						<view class="praise-me">
							点赞
						</view>
						<view :animation="animationData" class="praise-me animation-opacity">
							+1
						</view>						
					</view>
				</view>
			</view>
		</view>
		<!-- 猜你喜欢 end -->		
	</view>
</template>

<script>
	import common from "../../common/common.js";
	// 导入自定义组件
	import helloComp from "../../components/helloComp.vue";
	import trailerStars from "../../components/trailerStars.vue";
	export default {
		data() {
			return {
				title: 'Hello',
				carouselList:[],
				hotSuperheroList:[],
				hotTrailerList:[],
				
				animationData:{}
			}
		},
		onUnload(){
			// 页面卸载的时候，清除动画数据
			this.animationData = {};
		},
		onLoad() {
			var me = this;
			debugger;
			
			// 在页面创建的时候，创建一个临时的动画对象
			this.animation = uni.createAnimation();
			// 获取common.js中的服务器地址
			// var serverUrl = common.serverUrl;
			// 通过挂载到main.js中获取服务器的地址，作为全局变量
			var serverUrl = me.serverUrl;
			//请求轮播数据
			uni.request({
				url: serverUrl + '/index/carousel/list',
				method: "POST",
				header:{
					'content-type':'application/x-www-form-urlencoded'
				},
				data:{
					qq:'466481615'
				},
				success:(res) => {
					console.log(res.data);
					//获取真实数据之前，判断状态是否为200
					if(res.data.status == 200 ){
						var carouselList = res.data.data;
						this.carouselList = carouselList;
					}
					
					this.text = 'request success';
				}
			});
			
			// 查询超英热门
			uni.request({
				url: serverUrl + '/index/movie/hot?type=superhero',
				method: "POST",
				header:{
					'content-type':'application/x-www-form-urlencoded'
				},
				data:{
					qq:'466481615'
				},
				success:(res) => {
					//console.log(res.data);
					//获取真实数据之前，判断状态是否为200
					if(res.data.status == 200 ){
						var hotSuperheroList = res.data.data;
						this.hotSuperheroList = hotSuperheroList;
					}
					
					// this.text = 'request success';
				}
			});
			
			// 热门预告
			uni.request({
				url: serverUrl + '/index/movie/hot?type=trailer',
				method: "POST",
				header:{
					'content-type':'application/x-www-form-urlencoded'
				},
				data:{
					qq:'466481615'
				},
				success:(res) => {
					//console.log(res.data);
					//获取真实数据之前，判断状态是否为200
					// debugger;
					if(res.data.status == 200 ){
						var hotTrailerList = res.data.data;
						this.hotTrailerList = hotTrailerList;
					}
					
					// this.text = 'request success';
				}
			})
		},
		methods: {
			// 点赞动画效果
			praiseMe(){	
				// 构建动画数据，并且通过step来表示这组动画的完成
				this.animation.translateY(-60).opacity(1).step({
					duration:400
				});
				
				//导出动画数据到view组件，实现效果
				this.animationData = this.animation.export();
				
				//还原动画
				setTimeout(function(){
					this.animation.translateY(0).opacity(0).step({
					duration:0
					});
					this.animationData = this.animation.export();
				}.bind(this),500)
			}
		},
		
		components:{
			helloComp,
			trailerStars
		}
	}
</script>

<style>
	@import url("index.css");
	.content {
		text-align: center;
		height: 400upx;
	}

	.logo {
		height: 200upx;
		width: 200upx;
		margin-top: 200upx;
	}

	.title {
		font-size: 36upx;
		color: #8f8f94;
	}
</style>
