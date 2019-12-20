<template>
	<view class="Container1">
		<view class="card1">
			<view class="card_title">填写信息</view>
			<view class="card_content">
				<view class="form1">
					<text>
						<text style="color: red;">*</text>
						选手姓名
					</text>
					<input type="text" value="" v-model="name" placeholder="请输入选手姓名" />
				</view>
				<view class="form1">
					<text>
						<text style="color: red;">*</text>
						选手联系方式
					</text>
					<input type="number" value="" v-model="phone" placeholder="请输入选手联系方式" />
				</view>
				<view class="form1">
					<text>
						<text style="color: red;">*</text>
						选手年龄
					</text>
					<input type="number" value="" v-model="year" placeholder="请输入选手年龄" />
				</view>
				<view class="form1">
					<text>
						<text style="color: red;">*</text>
						所在学校
					</text>
					<input type="text" value="" v-model="school" placeholder="请输入选手所在学校" />
				</view>
				<view class="form1">
					<text>
						<text style="color: red;">*</text>
						所在班级
					</text>
					<input type="text" value="" v-model="banji" placeholder="请输入选手所在班级" />
				</view>
				
				
				<view class="tips">
					<view class="js_text">*上传3张选手海选展示图片</view>
			
				</view>
				<view class="chooseCont">
					<view class="addVideo" @tap="chooseVideo(1)">
						<image v-if="!img1" src="../../static/image/add.png" class="addImg" mode=""></image>
						<text v-if="!img1" class="addtext">添加图片</text>
						<image v-if="img1" :src="img1" class="adddimg" mode=""></image>
					</view>
					<view class="addVideo" @tap="chooseVideo(2)">
						<image v-if="!img2" src="../../static/image/add.png" class="addImg" mode=""></image>
						<text v-if="!img2" class="addtext">添加图片</text>
						<image v-if="img2" :src="img2" class="adddimg" mode=""></image>
					</view>
					<view class="addVideo" @tap="chooseVideo(3)">
						<image v-if="!img3" src="../../static/image/add.png" class="addImg" mode=""></image>
						<text v-if="!img3" class="addtext">添加图片</text>
						<image v-if="img3" :src="img3" class="adddimg" mode=""></image>
					</view>
				</view>
				<!-- <video class="addVideo" style="z-index: 1;" v-if="video_url" :src="video_url" @ended="playenda" controls></video> -->

				<view class="fenge"></view>

				<button class="f_btn" @tap="commit">提 交</button>
				
			</view>
		</view>
	</view>
</template>

<script>
import http from '../../common/getList.js';
export default {
	data() {
		return {
			name: '',
			phone: '',
			year: '',
			school:'',
			banji:'',
			city:'',
			xianqu:'',
			quyu:'',
			video_url: '',
			video1: '',
			videoList: [
				{
					url: '../../static/image/zhanshi.mp4',
					name: '案例视频参考',
					id: '0001'
				}
			],
			currentPlay: null,
			isplay: false,
			img1:'',    //选择图片本地地址
			img2:'',
			img3:'',
			imgurl1:'',
			imgurl2:'',  //上传返回之后的地址
			imgurl3:''
		};
	},
	methods: {
		commit(e) {
			var that = this;

			// if (that. === '') {
			// 	uni.showModal({
			// 		title: '温馨提示',
			// 		content: '请等待视频上传成功！'
			// 	});
			// }
			var myreg = /^(((13[0-9]{1})|(15[0-9]{1})|(18[0-9]{1})|(17[0-9]{1}))+\d{8})$/;
			if (!myreg.test(that.phone)) {
				uni.showModal({
					title: '温馨提示',
					content: '手机号有误！'
				});
			} else {
				let url = 'api/home/index/submitPost1';
				let data = {
					name: that.name,
					mobile: that.phone,
					age: that.year,
					school:that.school,
					banji:that.banji,
					pic_url1:that.imgurl1,
					pic_url2:that.imgurl2,
					pic_url3:that.imgurl3
				};
				http.request(url, data).then(
					res => {
						if (res.code === 1) {
							uni.showToast({
								title: '提交成功'
							});
							that.name = '';
							that.phone = '';
							that.year = '';
							that.school='';
							that.banji='';
							that.img1 = '';
							that.img2 = '';
							that.img3 = '';
						} else {
							uni.showModal({
								title: '温馨提示',
								content: res.msg
							});
						}
					},
					error => {}
				);
			}
		},
		chooseVideo(index) {
			var that = this;
			uni.chooseImage({
				count: 1,
				sourceType: ['album', 'camera'],
				sizeType:['compressed'],
				success: function(res) {
					console.log(res);
					if(index===1){
						that.img1 = res.tempFilePaths[0];
						that.uploadImg(that.img1,1)
					}else if(index===2){
						that.img2 = res.tempFilePaths[0];
						that.uploadImg(that.img2,2)
					}else if(index===3){
						that.img3 = res.tempFilePaths[0];
						that.uploadImg(that.img3,3)
					}
				}
			});
		},
		uploadImg(url,index){
			console.log(index);
			var that=this
			var uploadTask = uni.uploadFile({
				url: 'http://dongao.agampai.com/api/home/index/upload', //仅为示例，非真实的接口地址
				filePath: url,
				name: 'file',
				files: url,
				success(res) {
					console.log(res);
					if (res.data.code === 0) {
						uni.showModal({
							title: '温馨提示',
							content: res.msg
						});
					}
					if (res.statusCode === 200) {
						uni.showToast({
							title: '图片选择成功'
						});
						if(index===1){
							that.imgurl1 = res.data;
						}else if(index===2){
							that.imgurl2 = res.data;
						}else if(index===3){
							that.imgurl3 = res.data;
							console.log(that.imgurl3);
						}
					} else {
						uni.showToast({
							title: '图片选择失败！',
							icon: none
						});
					}
				},
				fail: error => {}
			});
			uploadTask.onProgressUpdate(res => {
				uni.showLoading({
					title: '图片上传中' + res.progress + '%'
				});
				if (res.progress == 100) {
					uni.hideLoading();
				}
			});
		},
		playvideo(e) {
			var index = e.currentTarget.dataset.index;

			this.currentPlay = index;
		},
		playend(e) {
			this.currentPlay = null;
		},
		playenda(e) {
			// var url = this.video_url;
			// this.video_url = '';
			// this.video_url = url;
		}
	}
};
</script>

<style>
	.adddimg{
		width: 200rpx;
	}
.chooseCont {
	display: flex;
	flex-direction: row;
	justify-content: space-between;
	width: 650rpx;
	flex-wrap: wrap;
}
.tips {
	width: 100%;
	margin-left: 40rpx;
	text-align: left;
}
.none1 {
	display: none !important;
}
.getImg {
	height: 600rpx;
	width: 350rpx;
}
.playtext {
	position: absolute;
	z-index: 999;
	width: 300rpx;
	margin-top: 50rpx;
	margin-left: 90rpx;
	font-size: 28rpx;
	color: gray;
}
.Container1 {
	display: flex;
	flex-direction: column;
	background-color: #007aff;
	padding-bottom: 60rpx;
	border-radius: 10rpx;
	margin: 0 30rpx;
	margin-top: 60rpx;
	z-index: 999;
}
.card1 {
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}
.card_title {
	color: white;
	text-align: center;
	height: 100rpx;
	line-height: 100rpx;
	letter-spacing: 5rpx;
	font-weight: 500;
}
.card_content {
	display: flex;
	flex-direction: column;
	align-items: center;
}
.form1 {
	margin-bottom: 20rpx;
	border-radius: 10rpx;
	background-color: white;
	width: 610rpx;
	padding: 20rpx;
	display: flex;
	flex-direction: row;
	align-items: center;
	justify-content: space-between;
	font-size: 32rpx;
}
.form1 input {
	margin-left: 50rpx;
	width: 340rpx;
	font-size: 32rpx;
}
.js_text {
	color: white;

	font-size: 34rpx;
}
.js_tips {
	font-size: 24rpx;
	color: white;
	margin-top: 10rpx;
}
.addVideo {
	width: 300rpx;
	height: 300rpx;
	background-color: white;
	margin-top: 20rpx;
	border-radius: 10rpx;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}
.addtext {
	font-size: 28rpx;
}
.f_btn {
	width: 650rpx;
	height: 90rpx;
	line-height: 90rpx;
	background-color: #ffcc19;
	color: #007aff;
	margin-top: 50rpx;
}
.fenge {
	width: 100%;
	height: 5rpx;
	background-color: white;
	margin-top: 100rpx;
}
.addImg {
	width: 40rpx;
	height: 40rpx;
}
.videoCont {
	display: flex;
	flex-direction: column;
	position: relative;
	width: 650rpx;
	height: 440rpx;
	margin: 20rpx;
	margin-top: 50rpx;
}
.coverCont {
	width: 650rpx;
	height: 440rpx;
	background-color: white;
	display: flex;
	align-items: center;
	justify-content: center;
	border-radius: 5rpx;
}
.coverImg {
	width: 650rpx;
	height: 370rpx;
}
.playicon {
	width: 60rpx;
	height: 60rpx;
	position: absolute;
}
.video1 {
	width: 650rpx;
	height: 440rpx;
	border-radius: 5rpx;
}
</style>
