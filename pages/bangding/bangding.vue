<template>
	<!-- <view class="container">
		<view class="item">
			<view class="label">
				<text>学号：</text>
			</view>
			<input type="no" value="" v-model="no" />
		</view>
		
	<view class="item">
		<view class="label">
			<text>密码：</text>
		</view>
		<input type="password" value="" v-model="password" />
	</view>
	 -->
	 
	 	<view class="load-main">
	 		<view class="load-box">
	 			<view class="load-text">
	 				教务绑定
	 			</view>
	 			<view class="load-input">
	 				<input placeholder-class="load-place" placeholder="请输入学号" v-model="no" />
	 			</view>
	 			<view class="load-input">
	 				<input type="password" placeholder-class="load-place" placeholder="请输入密码" v-model="password" />
	 			</view>
	 			<view>
	 				<view class="load-button">
	 					<button class="load-button-style"  @tap="bindApiRequest">绑定</button>
	 				</view>
	 			</view>
	 		</view>
	 	</view>
	 
	
</template>

<script>
	export default {
		data() {
			return {
				userInfo: uni.getStorageSync('userInfo'),
				no:'',
				password:''
			}
		},
		methods: {
			// gonavigate() {
			// 	uni.navigateTo({
			// 		url: '/pages/detail/detail'
			// 	})
			// },
			// goregirect() {
			// 	uni.redirectTo({
			// 		url: '/pages/about/about'
			// 	});

			// },
			// goopenurl() {
			// 	void plus.runtime.openURL("http://www.baidu.com")
			// }
			//绑定教务
			bindApiRequest() {
				
						// 绑定接口
						uni.request({
							url: 'https://openapi.hackerxiao.online/api/v1/edu/bind',
							method: 'POST',
							header: {
								Authorization: 'Bearer ' + this.userInfo.token,
								'Content-Type': 'application/json',
							},
							data: {
								password: this.password,
								studentID: this.no,
							},
							success: res => {
								if (res.data.code !== 1000) {
									uni.showToast({
										title: JSON.stringify(res.data.msg),
										icon: 'error',
									})
									return
								}
								if(res.data.code='1000')
								uni.showToast({
									title: '绑定成功',
								
								});
								uni.switchTab({
									url:'/pages/user/user'
								})
								console.log('Request successful');
								console.log(res);
							}
						});
			
					
			},
		}
	}
</script>

<style>
	.load-main {
		display: flex;
		justify-content: center;
		 background-color: white; 
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