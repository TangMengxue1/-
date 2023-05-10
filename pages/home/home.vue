<template>
<view>
	<view class="swiper">
		<swiper class="swiper" :current="1" :circular="true" :vertical="true"
		@change="changefun" @animationfinish="animationfinishfun" >
			<swiper-item v-for="(item,index) in PayVideo">
				<view class="swiper-item">
					<video class="video" :id="'id'+index" loop="true" autoplay 
					:src="item.video_path" :custom-cache="false" :controls="false" 
					:enable-play-gesture="true" :enable-progress-gesture="true" 
					:show-center-play-btn="false" ></video>
				</view>
			</swiper-item>
		</swiper>
	</view>
	<view v-if="is_active">
		<view class="left">
			<cover-view class="left_box" style="margin-bottom:150rpx;">
				<cover-view class="ren">@汤圆</cover-view>
				<cover-view class="ke_context">蓝蓝天空，青青草地</cover-view>
			</cover-view>
		</view>
		<view class="right">
			<cover-view class="right_box">
				<cover-view class="top1">
					<cover-image class="avatar_img" src="https://p1.itc.cn/q_70/images03/20211118/1e16d066aedc4065aab0b6c2067e30a8.jpeg" mode="aspectFill"></cover-image>
					<cover-image class="add_img" src="../../static/imgs/加.png" mode="aspectFill"></cover-image>
				</cover-view>
				<cover-view @click="like_change" class="top2">
					<cover-image v-if="!PayVideo[index_].like" class="t_img" 
					src="../../static/imgs/213喜欢.png" mode="aspectFill"></cover-image>
					<cover-image v-else class="t_img" 
					src="../../static/imgs/213喜欢 (1).png" mode="aspectFill"></cover-image>
					<cover-view class="font_t">{{PayVideo[index_].like_num}}</cover-view>
				</cover-view>
				<cover-view class="top2">
					<cover-image class="t_img" src="../../static/imgs/抖音-评论.png" mode="aspectFill"></cover-image>
					<cover-view class="font_t">0</cover-view>
				</cover-view>
				<cover-view class="top2">
					<cover-image class="t_img" src="../../static/imgs/图标_界面_抖音转发.png" mode="aspectFill"></cover-image>
					<cover-view class="font_t">0</cover-view>
				</cover-view>
			</cover-view>
		</view>
	</view>
</view>
</template>

<script>
export default {
	data() {
		return {
			data: [
				{ "video_path": '../../static/video/实拍草原羊群素材.mp4',
				  "like": false, "like_num": 0 },
				{ "video_path": '../../static/video/实拍海岛素材.mp4',
				  "like": false, "like_num": 1 },
				{ "video_path": '../../static/video/实拍荒山上车辆旅行风景.mp4',
				  "like": false, "like_num": 2 }, 
				{ "video_path": '../../static/video/天空云朵实拍.mp4',
				  "like": false, "like_num": 3 }, 
				{ "video_path": '../../static/video/紫霞云朵树林实景拍摄素材.mp4',
				  "like": false, "like_num": 4 },
			],
			index_: 1,
			index: '1',
			is_active: true,
			active: 2,
			PayVideo: [],
			_arr: [],
			len: 0,
		}
	},
	methods: {
		like_change() {
			let obj = this.PayVideo[this.index_]
			obj.like = !obj.like
			obj.like == true ? obj.like_num=parseInt(obj.like_num) + 1 : obj.like_num=parseInt(obj.like_num) - 1
		},
		changefun(e) {
			this.is_active = false
			let current = e.detail.current
			let { len, PayVideo, active,
				_arr, t, index_ } = this
			let videoContext = uni.createVideoContext('id' + index_)
			videoContext.pause()
			this.PayVideo = PayVideo
		},
		animationfinishfun(e) {
			let { index_, len, PayVideo,
				active, _arr, t } = this
			let current = e.detail.current
			this.is_active = true
			PayVideo[current]['istrue'] = true
			this.PayVideo = PayVideo
			let videoContext = uni.createVideoContext('id' + index_)
			videoContext.pause()
			videoContext = uni.createVideoContext('id' + current)
			videoContext.play()
			this.index_ = current
			if (PayVideo.length == len) {
				return
			}
			this.PayVideo.push(_arr[active])
			this.active += 1
		}
	},
	mounted() {
		uni.setStorageSync('video', this.data);
		let ind = 1
		this.index = ind
		const video = uni.getStorageSync('video');
		const len = video.length
		let b_arr = []
		let s_arr = []
		video.forEach((res, index) => {
			if (ind <= index) {
				b_arr.push(res)
			} else {
				s_arr.push(res)
			}
		})
		let _arr = b_arr.concat(s_arr.reverse())
		_arr = _arr.map(res => {
			res['istrue'] = false
			return res
		})
		_arr[0]['istrue'] = true
		const PayVideo = [
			_arr[len - 1], _arr[0], _arr[1]
		]
		this.PayVideo = PayVideo
		this._arr = _arr
		this.len = len
		this.$nextTick(function() {
			let videoContext = uni.createVideoContext('id1')
			videoContext.play()
		})
	}
}
</script>

<style scoped lang="less">
page{ font-family: PingFang SC; }
.swiper {
	height: 100vh;
	.swiper-item {
		position: relative;
		height: 100vh;
		background-color: #ffffff;
	}
}
.video {
	position: relative;
	width: 100%;
	height: 100vh;
}
.left_box {
	position: fixed;
	bottom: 60rpx;
	left: 30rpx;
	width: 516rpx;
	color: #FFFFFF;
	.ke {
		display: flex;
		align-items: center;
		cover-view { font-size: 26rpx; }
		cover-image {
			width: 40rpx;
			height: 40rpx;
			margin-right: 15rpx;
		}
	}
	.ren {
		font-size: 36rpx;
		font-weight: bold;
		margin: 20rpx 0;
	}
	.ke_context {
		font-size: 30rpx;
		word-break: break-all;
		word-wrap: break-word;
		white-space: pre-line;
		white-space: normal;
	}
	.auto {
		display: flex;
		align-items: center;
		margin-top: 24rpx;
		cover-view { font-size: 26rpx; }
		cover-image {
			width: 26rpx;
			height: 28rpx;
			margin-right: 10rpx;
		}
	}
}
.right_box {
	width: 100rpx;
	position: absolute;
	bottom: 60rpx;
	right: 12rpx;
	display: flex;
	flex-direction: column;
	color: #FFFFFF;
	.top1 {
		.avatar_img {
			width: 88rpx;
			height: 88rpx;
			border: 3px solid #FFFFFF;
			border-radius: 50%;
		}
		.add_img {
			width: 48rpx;
			height: 48rpx;
			margin: -20rpx auto 0;
		}
	}
	.top2 {
		text-align: center;
		margin-top: 37rpx;
		.t_img {
			height: 72rpx;
			width: 72rpx;
			margin: 0 auto 10rpx;
		}
		.font_t { font-size: 26rpx; }
	}
}
</style>


