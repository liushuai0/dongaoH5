<template>
	<view class="_notice">
		<swiper class="_swiper tc" @change="slideChange" indicator-dots="false" autoplay="true" :interval="interval" circular="true"
		 display-multiple-items="2" :duration="duration">
			<swiper-item style="width: 750rpx;">
				<view class="swiper-item uni-bg-red"></view>
			</swiper-item>
			<swiper-item>
				<view class="swiper-item uni-bg-red"></view>
			</swiper-item>
			<swiper-item v-for="(item,index) in list" :key="index">
				<view class="swiper-item uni-bg-red">{{item}}</view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default {
		props: {
			//显示数据
			list: {
				type: Array,
				defual:function(){
					return [];
				}
			},
		},
		data() {
			return {
				interval:3000,
				duration:12000,
			};
		},
		methods:{
			slideChange(e){
				this.$emit('getCurrentIndex',e.detail.current); 
				
			}
		}
	};
</script>

<style lang="scss">
	._notice {
		position: fixed;
		left: 0;
		top: 0;
		/* #ifdef H5 */
		top: 88upx;
		/* #endif */
		z-index: 9;
		width: 100%;
		background: rgba(72, 1, 1, 0.3);
		font-size: 20upx;
		height: 44upx;
		color: #ffffff;
		border-radius: 6upx;
		overflow: hidden;
		box-sizing: border-box;
	}
	._swiper {
		line-height: 44upx;
	}
</style>