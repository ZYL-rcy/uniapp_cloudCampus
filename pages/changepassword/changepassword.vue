<template>
	
	<view class="load-main">
		<view class="load-box">
			<view class="load-text">
				修改密码
			</view>
			<view class="load-input">
				<input placeholder-class="load-place" placeholder="请输入邮箱" v-model="email" />
			</view>
			<view class="load-input">
				<input type="password" placeholder-class="load-place" placeholder="旧密码" v-model="password" />
			</view>
			<view class="load-input">
				<input type="password" placeholder-class="load-place" placeholder="新密码" v-model="newpassword" />
			</view>
			<view class="load-input">
				<input type="password" placeholder-class="load-place" placeholder="确认新密码" v-model="renewpassword" />
			</view>
			<view>
				<view class="load-button">
					<button class="load-button-style"  @tap="changePassword">确认修改</button>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				newpassword:'',
				password:'',
				renewpassword:'',
				email:'',
			}
		},
		methods: {
				changePassword(){
					uni.request({
						url:'https://openapi.hackerxiao.online/api/v1/auth/resetpass',
						method:'POST',
						data:{
							email:this.email,
							password:this.password,
							renewpassword:this.renewpassword,
							newpassword:this.newpassword
						},
						success: (res) => {
							if(res.data.code='1000')
							uni.showToast({
								title: '修改成功',
							
							});
							uni.switchTab({
								url:'/pages/user/user'
							})
							console.log('Request successful');
							console.log(res);
						}
					})
				}
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