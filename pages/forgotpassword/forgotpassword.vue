<template>
	
<view class="load-main">
		<view class="load-box">
			<view class="load-text">
				找回密码
			</view>
			<view class="load-input">
				<input placeholder-class="load-place" placeholder="请输入邮箱" v-model="username" />
			</view>
			<view class="load-input">
				<input placeholder-class="load-place" placeholder="请输入验证码" v-model="captcha" />
					<button type="default" class="yzm-btn" v-bind:disabled="btndisabled" @tap='getyzm'>{{btnstr}}</button>
					
			</view>
			<view class="load-input">
				<input type="password" placeholder-class="load-place" placeholder="新密码" v-model="newpassword" />
			</view>
			<view class="load-input">
				<input type="password" placeholder-class="load-place" placeholder="确认新密码" v-model="renewpassword" />
			</view>
			<view>
				<view class="load-button">
					<button class="load-button-style" @tap="forgotPassword">确认修改</button>
				</view>
			</view>
		</view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				username: '',
				newpassword: '',
				captcha: '',
				renewpassword: '',
				yzmstr: '获取验证码',
				btnstr: '获取验证码',
				btndisabled: false
			}
		},
		onLoad() {
			
		},
		methods: {
			timewait(t) {
				const _this = this
				setTimeout(function() {
					if (t >= 0) {
						_this.btnstr = t + '秒后点击';
						t--;
						_this.timewait(t);
					} else {
						_this.btnstr = '获取验证码';
						t = 60;
						_this.btndisabled = false
					}
				}, 1000)
			},
			//获取验证码
			getyzm() {
				uni.request({
					url: 'https://openapi.hackerxiao.online/api/v1/auth/sendemail?email=' + this.username +
						'&mode=recoverpass',
					method: 'GET',

					data: {
						email: this.username,
						mode: 'recoverpass'
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: res => {
						if (res.code = 1000) {
							uni.showToast({
								title: '已成功发送',
								
							})



						}

						console.log(res);
					},
					fail: () => {},
					complete: () => {}
				});

				this.timewait(60);
				this.btndisabled = true;
			},
			forgotPassword(){
				uni.request({
					url:'https://openapi.hackerxiao.online/api/v1/auth/recoverpass',
					method:'POST',
					data:{
						captcha:this.captcha,
						newpassword:this.newpassword,
						renewpassword:this.renewpassword,
						email:this.username
					},
					success: res => {
						if (res.data.msg = 1000) {
							uni.showToast({
								title: '此邮箱密码已被找回',
								icon: 'success',
							});
							uni.switchTab({
								url:'/pages/login/login'
							})
					}
					}
				})
			}
			
		}
	}
</script>

<style scoped>
	
	.load-main {
		display: flex;
		justify-content: center;
		 background-color: white; 
	}
	
	.load-box {
		display: flex;
		width: 90%;
		height: 400px;
		flex-direction: column;
		justify-content: space-between;
	}
	
	.load-text {
		line-height: 40px;
		font-size: 25px;
	}
	
	.load-input {
		display: flex;
		border: 1px solid #cccccc;
		height: 60px;
		align-items: center;
	}
	
	.load-place {
		padding-left: 10px;
		font-size: 15px;
	}
	.load-button-style {
	    background-color: green;
	    color: white;
	    border-radius: 8px; /* 添加圆角 */
	    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2); /* 添加阴影效果 */
	}
	
	.load-button{
		background-color: green;
		color: white; /* 文本颜色设置为白色以确保对比度 */
	}
	
</style>