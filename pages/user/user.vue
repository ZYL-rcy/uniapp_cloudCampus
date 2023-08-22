<template>

	<view class="center">
		
			<view class="logo" @click="goLogin" :hover-class="!login ? 'logo-hover' : ''">
				<image class="logo-img" :src="login ? avatarUrl : avatarUrl"></image>
				<view class="logo-title">
					<text class="uer-name">Hi，{{login ? userInfo.username : '欢迎登录！'}}</text>
					<text class="go-login navigat-arrow" v-if="!login">&#xe65e;</text>
				</view>
			</view>
			<view class="center-list">
				<view class="center-list-item border-bottom">
					<text class="list-icon">&#xe60f;</text>
					<text class="list-text" @tap="changePassword">修改密码</text>
					<text class="navigat-arrow">&#xe65e;</text>
				</view>
				<view class="center-list-item">
					<text class="list-icon">&#xe639;</text>
					<text class="list-text" @tap="getcook">教务通知</text>
					<text class="navigat-arrow">&#xe65e;</text>
				</view>
			</view>
			<view class="center-list">
				<view class="center-list-item border-bottom">
					<text class="list-icon">&#xe60b;</text>
					<text class="list-text" @tap="bangding">绑定教务</text>
					<text class="navigat-arrow">&#xe65e;</text>
				</view>
				<view class="center-list-item">
					<text class="list-icon">&#xe65f;</text>
					<text class="list-text" @tap="bindApiRequest">服务条款及隐私</text>
					<text class="navigat-arrow">&#xe65e;</text>
				</view>
			</view>
			<view class="center-list">
				<view class="center-list-item">
					<text class="list-icon">&#xe614;</text>
					<text class="list-text" @tap="logout">退出账号</text>
					<text class="navigat-arrow">&#xe65e;</text>
				</view>
			</view>
		</view>
		
		
	 
</template>

<script>
	export default {
		data() {
			return {
				userInfo: uni.getStorageSync('userInfo'),
				avatarUrl: "../../static/uni-center/logo.png",
				loadtype: 'main',
				email:'',
				password:'',
				
			}
		},

		computed: {
			login() {
				return typeof uni.getStorageSync('userInfo') == 'object'
			},
			

		},

		methods: {
			goLogin() {
				if (!this.login) {
					uni.navigateTo({
						url: '/pages/login/login',
						success(e) {
							console.log(e);
						},
						fail(e) {
							console.log(e);
						}
					});
				}
			},
			//返回主界面
			main() {
				this.loadtype = 'main'
			},
			//绑定界面
			bangding() {
				this.loadtype = 'bd'
			},
			logout() {
				uni.removeStorageSync('username');
				uni.removeStorageSync('password');
				uni.showToast({
					title: '退出成功！'
				});
				uni.redirectTo({
					url: '/pages/login/login'
				})

			},
			bangding(){
					
					uni.navigateTo({
						url: '/pages/bangding/bangding',
						success(e) {
							console.log(e);
						},
						fail(e) {
							console.log(e);
						}
					});
			},
			changePassword(){
				uni.navigateTo({
					url: '/pages/changepassword/changepassword',
					success(e) {
						console.log(e);
					},
					fail(e) {
						console.log(e);
					}
				});
			},
			//绑定教务
			bindApiRequest() {
				uni.getStorage({
					key: 'token',
					success: function(res) {
						const token = res.data;
						// 绑定接口
						uni.request({
							url: 'https://openapi.hackerxiao.online/api/v1/edu/bind',
							method: 'POST',
							header: {
								Authorization: 'Bearer ' + token,
								'Content-Type': 'application/json',
							},
							data: {
								password: 'Zyl051811',
								studentID: '2203050232'
							},
							success: res => {

								console.log('Request successful');
								console.log(res);
							}
						});

					}
				});
			},
			//获取cookie
			// getcook() {
			// 	uni.getStorage({
			// 		key: 'token',
			// 		success: function(res) {
			// 			const token = res.data;
			// 			uni.request({
			// 				url: 'https://openapi.hackerxiao.online/api/v1/edu/cookie',
			// 				method: 'GET',
			// 				header: {
			// 					Authorization: 'Bearer ' + token,
			// 					'Content-Type': 'application/json',
			// 				},
			// 				data: {

			// 				},
			// 				success: (res) => {
			// 					uni.setStorage({
			// 						key: 'cookie',
			// 						data: res.data.data,
			// 						success: () => {
			// 							console.log('cookie saved successfully');

			// 						}
			// 					});
			// 					console.log(res.data.data);

			// 				}
			// 			});
			// 		}
			// 	});

			// },



		}
	}
</script>

<style>
	@font-face {
		font-family: texticons;
		font-weight: normal;
		font-style: normal;
		src: url('https://at.alicdn.com/t/font_984210_5cs13ndgqsn.ttf') format('truetype');
	}

	page,
	view {
		display: flex;
	}

	page {
		background-color: #f8f8f8;
	}

	.center {
		flex-direction: column;
	}

	.logo {
		width: 750upx;
		height: 240upx;
		padding: 20upx;
		box-sizing: border-box;
		background-color: #658f69
		;
		flex-direction: row;
		align-items: center;
	}

	.logo-hover {
		opacity: 0.8;
	}

	.logo-img {
		width: 150upx;
		height: 150upx;
		border-radius: 150upx;
	}

	.logo-title {
		height: 150upx;
		flex: 1;
		align-items: center;
		justify-content: space-between;
		flex-direction: row;
		margin-left: 20upx;
	}

	.uer-name {
		height: 60upx;
		line-height: 60upx;
		font-size: 38upx;
		color: #FFFFFF;
	}

	.go-login.navigat-arrow {
		font-size: 38upx;
		color: #FFFFFF;
	}

	.login-title {
		height: 150upx;
		align-items: self-start;
		justify-content: center;
		flex-direction: column;
		margin-left: 20upx;
	}

	.center-list {
		background-color: #FFFFFF;
		margin-top: 20upx;
		width: 750upx;
		flex-direction: column;
	}

	.center-list-item {
		height: 90upx;
		width: 750upx;
		box-sizing: border-box;
		flex-direction: row;
		padding: 0upx 20upx;
	}

	.border-bottom {
		border-bottom-width: 1upx;
		border-color: #c8c7cc;
		border-bottom-style: solid;
	}

	.list-icon {
		width: 40upx;
		height: 90upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #4cd964;
		text-align: center;
		font-family: texticons;
		margin-right: 20upx;
	}

	.list-text {
		height: 90upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #555;
		flex: 1;
		text-align: left;
	}

	.navigat-arrow {
		height: 90upx;
		width: 40upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #555;
		text-align: right;
		font-family: texticons;
	}
	.load-box {
		display: flex;
		width: 90%;
		height: 250px;
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
		height: 40px;
		align-items: center;
	}
	
	.load-place {
		padding-left: 10px;
		font-size: 15px;
	}
</style>
<!-- <template>
	<view class="wrap">
		<view class="uni-form-item uni-column" v-if="active===1">
			<view class="title">用户登录</view>
			<view class="ipt">
				<text>账号</text>
				<input class="uni-input" name="input" placeholder="请输入账号.." v-model="phone"/>
			</view>
			<view class="ipt">
				<text>密码</text>
				<input class="uni-input" name="input" placeholder="请输入密码.." v-model="pwd"/>
			</view>
			<view class="btn"> 
				<button size="mini" @click="login">登录</button>
				<button size="mini" @click="active=3">注册</button>
			</view>
		</view>
		<view class="my" v-if="active===2">
			<view class="top">
				<image src="https://m.21cake.com/wap_themes/21cake/images/icon/ico_member.png"></image>
				<view>{{userInfo.nickname}}</view>
				<view>
					手机:{{userInfo.phone}}
				</view>
			</view>
		</view>
		<view class="uni-form-item uni-column" v-if="active===3">
			<view class="title">用户注册</view>
			<view class="ipt">
				<text>账号</text>
				<input class="uni-input" name="input" placeholder="请输入账号.." v-model="phone"/>
			</view>
			<view class="ipt">
				<text>密码</text>
				<input class="uni-input" name="input" placeholder="请输入密码.." v-model="pwd" />
			</view>
			<view class="ipt">
				<text>昵称</text>
				<input class="uni-input" name="input" placeholder="请输入密码.." v-model="nickname"/>
			</view>
			<view class="btn"> 
				<button size="mini" @click="reg">确认</button>
				<button size="mini" @click="active=1">返回</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				phone: "",
				pwd: "",
				nickname: "",
				active: 1,
				userInfo: {
					phone:"",
					nickname:""
				}
			}
		},
		onShow() {
			
		},
		methods: {
			reg(){
				
				
			},
			login(){
				var data = {
					phone:"15201331139",
					pwd:"123456",
				}
				
			}
		}
	}
</script>


<style lang="scss" scoped>
	.wrap{
		color: #B2B2B2;
		.ipt{
			background:#FFF;
			width: 90%;
			margin: 0 auto;
			text{
				vertical-align: 20rpx
			}
			input{
				display: inline-block;
				border-bottom:1px solid #B2B2B2;
			}
			.btn{
				text-align: center;
			}
		}
		.my{
			border:1px solid #8e6a55;
			border-radius: 4rpx;
			width: 98%;
			margin: 0 auto;
			.top{
				text-align: center;
				image{
					border-radius: 50%;
					width: 100rpx;
					height: 100rpx;
					border:1px solid #8e6a55;
					display: inline-block;
				}
			}
			.bottom{
				display: flex;
				view{
					flex: 1;
					height: 50rpx;
					border:1px solid #8e6a55;
				}
			}
		}
	}
</style -->>