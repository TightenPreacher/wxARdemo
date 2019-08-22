<template>
	<view class="center" style="width: 100%;">
        <remote-js src="//apps.8thwall.com/xrweb?appKey=vLbq5m5zCKxmhV33fMDDy4cuLXb9pxy0AvhdgANjfQUDDjnFYfbnSnsG6z2FjoBI6U63RL&"></remote-js>
		<camera device-position="back" frame-size="small" flash="off" @error="error" style="width: 100%; height: 300px;"></camera>
        <!-- <button type="primary" @click="takePhoto">拍照</button>
        <view>预览</view>
        <image mode="widthFix" :src="src"></image> -->
        <canvas canvas-id="came" style="width: 100%; height: 300px;display:block" @error="canvasIdErrorCallback"></canvas>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				login: false,
				avatarUrl: '/static/logo.png',
				uerInfo: {}
			}
        },
        onShow: function (e) {
            this.getCamer();
			console.log(window);
			console.log(document);
			console.log(global);
        },
		methods: {
            canvasIdErrorCallback: function (e) {
                console.error(e.detail.errMsg)
            },
			goLogin() {
				if (!this.login) {
					console.log('点击前往登录');
				}
			},
			takePhoto() {
                const ctx = uni.createCameraContext();
                ctx.takePhoto({
                    quality: 'high',
                    success: (res) => {
                        this.src = res.tempImagePath
                    }
                });
            },
            error(e) {
                console.log(e.detail);
            },
            getCamer(e) {
                let context = uni.createCameraContext();
                let listener = context.onCameraFrame((frame) => {
                    uni.canvasPutImageData({
                        canvasId: 'came',
                        x: 0,
                        y: 0,
                        width: frame.width,
                        heihgt: frame.heihgt,
                        data: new Uint8ClampedArray(frame.data),
                        success(res) {
                            console.log('OK')
                        }
                    })
                });
                listener.start();
            }
		}
	}
</script>

<style>
	page {
		min-height: 100%;
		background-color: #FFFFFF;
	}
</style>
