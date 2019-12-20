<template>
	<!-- 变相实现插槽 -->
	<view class="imageContent">
		<!-- 判断自定义galleryId -->
		<block v-if="galleryId === 'test'">
			<view class="num" v-if="scope.type==='num'">
				{{scope.content}}
			</view>
			<view class="content" v-else>
				{{scope.content}}
			</view>
		</block>
	</view>
</template>

<script>
	export default {
		props: {
			scope: {	//数据
				type: [Object, String],
				default() {
					return {};
				}
			},
			galleryId: {	//自定义标识
				type: [Object, String],
				default: ''
			}
		}
	}
</script>

<style scoped>
	.imageContent{
		height: 100%;
		width: 100%;
	}
	.imageContent .content{
		height: 20%;
		width: 100%;
		position: absolute;
		bottom: 0;
		left: 0;
		background-color: rgba(0,0,0,.4);
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		color: white;
		font-size: 28rpx;
	}
	.imageContent .num{
		height: 50rpx;
		width: 50rpx;
		position: absolute;
		top: 0;
		right: 0;
		background-color: rgba(0,0,0,.4);
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		color: white;
		font-size: 28rpx;
	}
</style>
