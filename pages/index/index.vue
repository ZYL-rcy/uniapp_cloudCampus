<template>
	<view class="container">
		<view class="header">
			<view class="score-types">
				<view v-for="item in scoreItems" :key="item.type" class="score-type"
					:class="{'active': type === item.type}" @click="changeType(item.type)">
					{{ item.label }}
				</view>
			</view>
		
		<view class="score-term">
		  <picker @change="bindPickerChange" :value="index" :range="array">
		    <view class="picker">{{ array[index] }}</view>
		  </picker>
		</view>

	</view>
	<view class="score-list">
		<view class="term-name" v-model="list.year"> {{list.year}}{{list.semester}}</view>
		<view class="list">
			<view class="score-item" v-for="(list,index) in list.gradesList">
				<view class="course-name">
				<view class="course-name">{{list.name}}</view>
				<view class="course-name" :class="{'raw-score_content': type === 2}">
					<view v-if="type === 2" >学位课：{{ list.isdegree }}</view>
					        <view v-if="type === 2">学分：{{ list.credits }} 绩点：{{ list.gpa }}</view>
					      
				</view>
				
			</view>
			<view class="course-score" >{{list.grade}}</view>
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
				password: '',
				cookie:uni.getStorageSync('cookie'),
				scoreItems: [{
						type: 1,
						label: '原始成绩'
					},
					{
						type: 2,
						label: '详细'
					}
				],
				type: 1, //有效成绩是1 ，原始成绩是2
				list:[],  //成绩列表
				array: ['2022-2023学年第1学期', '2022-2023学年第2学期', '2023-2024学年第1学期','2023-2024学年第2学期'],
				yearList:[2022,2022,2023,2023,2024],
				semesterList:[3,12,3,12],
				index: 0,
				year: 2022, //学年2022的化就是2022-2023学年
				semester: 3,
				//学期，3为第一学期，12为第二学期

			}
		},
		created() {
		    // This method will be called when the page is created
			
		    this.getGrade();
			
		  },
		  onLoad() {
		  	this.getGrade();
		  },
		  
		methods: {
			changeType(selectedType) {
				this.type = selectedType;
			},
			bindPickerChange(e) {
			  this.index = e.detail.value; // 更新索引
			 this.semester = (this.index + 1) % 2 === 1 ? 3 : 12;
			 
			  this.year=this.yearList[e.detail.value];
			  console.log(this.semester);
			  console.log(this.year);
			  
			  //this.list = uni.getStorageSync('grade').data;
			  
			  this.getGrade();
			  
			},
			//获取学期
			getSemester(){
				uni.request({
					url:'https://openapi.hackerxiao.online/api/v1/edu/semester',
					method:'GET',
					header: {
						Authorization: 'Bearer ' + uni.getStorageSync('userInfo').token,
						'Content-Type': 'application/json',
					},
					data:{},
					success:(res)=> {
						uni.setStorage({
							key: 'semester',
							data: res.data.data.list,
							success: () => {
								console.log(res.data.data.list);
								console.log('Semester saved successfully');
							}
						});
					}
				})
			},
			getGrade(){
				uni.request({
					url:'https://openapi.hackerxiao.online/api/v1/edu/grades',
					method:'POST',
					header: {
						Authorization: 'Bearer ' + uni.getStorageSync('userInfo').token,
						'Content-Type': 'application/json',
					},
					data:{
						cookie:this.cookie,
						Semester:this.semester,
						Year:this.year,
					},
					success:(res)=> {
						console.log(res.data);
						this.list=[];
						uni.setStorage({
							key: 'grade',
							data: res.data,
							success: () => {
								
								console.log('Grade saved successfully');
							}
						});
						this.list = uni.getStorageSync('grade').data;
					}
				})
			},
		},


	}
</script>

<style>
	page {
	  background: linear-gradient(
	    0deg,
	    rgba(161, 140, 209, 0.2) 0%,
	    rgba(251, 194, 235, 0.2) 100%
	  );
	}
	
	.container {
	  padding: 20rpx;
	}
	
	.header {
	  display: flex;
	  align-items: center;
	  justify-content: space-between;
	}
	
	.header>view {
	  width: calc((100% - 25rpx) / 2);
	  height: 100rpx;
	  border-radius: 20rpx;
	  background: rgba(255, 255, 255, 0.52);
	  box-shadow: 0px 0px 10px -20px rgba(0, 0, 0, 0.05);
	  display: flex;
	  align-items: center;
	  justify-content: center;
	  font-size: 15px;
	  color: #3b3b3b;
	}
	
	.score-type {
	  width: calc((100% - 20rpx) / 2);
	  height: 70rpx;
	  border-radius: 20rpx;
	  background: rgba(230, 230, 230, 0.8);
	  box-shadow: 0px 0px 20px -20px rgba(0, 0, 0, 0.05);
	  display: flex;
	  align-items: center;
	  justify-content: center;
	}
	
	.score-types {
	  padding: 10rpx 10rpx;
	  display: flex;
	  align-items: center;
	  justify-content: space-between;
	}
	
	.active {
	  background: #658f69; /* Light green color */
	  color: #ffffffff; /* White color */
	}
	
	.score-term {
	  padding: 8rpx 10rpx;
	  display: flex;
	  align-items: center;
	  justify-content: space-between;
	  font-size: 22rpx;
	  color: #3b3b3b;
	}
	
	.score-list {
	  width: 100%;
	  border-radius: 20rpx;
	  background: rgba(230, 230, 230, 0.8);
	  box-shadow: 0px 0px 10px -20px rgba(0, 0, 0, 0.05);
	  margin-top: 26rpx;
	  padding: 10rpx;
	}
	
	.term-name {
	  font-size: 32px;
	  color: #5555555;
	  text-align: center;
	}
	
	.list {
	  margin-top: 30rpx;
	}
	
	.score-item {
	  display: flex;
	  align-items: center;
	  justify-content: space-between;
	  min-height: 70rpx;
	  margin-top: 12rpx;
	}
	
	.score-item:not(:last-child) {
	  border-bottom: 1rpx solid rgba(232, 224, 235, 0.5);
	}
	
	.course-name {
	  font-size: 32rpx;
	  color: #3333333;
	}
	
	.course-score {
	  font-size: 32rpx;
	  color: #658f69 /* Light green color */
	}
	
	.fail {
	  color: #9c9c9c;
	}
	
	.raw-score_content {
	  font-weight: 400;
	  font-size: 24rpx;
	  color: #999999;
	}

</style>
<!-- <template>
  <view class="container">
    <view class="header">
      <view class="score-types">
        <view v-for="item in scoreItems" :key="item.type" class="score-type" :class="{'active': type === item.type}" @click="changeType(item.type)">
          {{ item.label }}
        </view>
      </view>

      <view class="score-term">
        <picker @change="bindPickerChange" :value="index" :range="array">
          <view class="picker">{{ array[index] }}</view>
        </picker>
      </view>
    </view>

    <view class="score-list">
      <view class="term-name"> {{selectedYear}} {{selectedSemester}}</view>
      <view class="list">
        <view class="score-item" v-for="(item, index) in list.gradesList" :key="index">
          <view class="course-name">
            <view>{{ item.name }}</view>
            <view class="course-name" :class="{'raw-score_content': type === 2}">
              <view v-if="type === 2">学位课：{{ item.isdegree }}</view>
              <view v-if="type === 2">学分：{{ item.credits }} 绩点：{{ item.gpa }}</view>
            </view>
          </view>
          <view class="course-score">{{ item.grade }}</view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      scoreItems: [
        { type: 1, label: '原始成绩' },
        { type: 2, label: '详细' }
      ],
      type: 1, // 有效成绩是1，原始成绩是2
      list: { gradesList: [] }, // 初始为空
      array: ['2022-2023学年第1学期', '2022-2023学年第2学期', '2023-2024学年第1学期', '2023-2024学年第2学期'],
      yearList: [2022, 2022, 2023, 2023, 2024],
      semesterList: [3, 12, 3, 12],
      index: 0,
      selectedYear: '',
	  cookie:uni.getStorageSync('cookie'),
      selectedSemester: ''
    };
  },
  created() {
    this.getGrade();
  },
  methods: {
    changeType(selectedType) {
      this.type = selectedType;
    },
    bindPickerChange(e) {
      this.index = e.detail.value;
      this.selectedSemester = (this.index + 1) % 2 === 1 ? '第1学期' : '第2学期';
      this.selectedYear = `${this.yearList[e.detail.value]}-${this.yearList[e.detail.value] + 1}学年`;
      this.getGrade();
    },
    getGrade() {
      uni.request({
        url: 'https://openapi.hackerxiao.online/api/v1/edu/grades',
        method: 'POST',
        header: {
          Authorization: 'Bearer ' + uni.getStorageSync('userInfo').token,
          'Content-Type': 'application/json'
        },
        data: {
          cookie: this.cookie,
          Semester: this.selectedSemester === '第1学期' ? 3 : 12,
          Year: this.yearList[this.index]
        },
        success: (res) => {
          console.log(res.data);
          this.list = res.data;
          uni.setStorage({
            key: 'grade',
            data: res.data,
            success: () => {
              console.log('Grade saved successfully');
            }
          });
        }
      });
    }
  }
};
</script>

<style>
/* 样式部分不变 */
page {
	  background: linear-gradient(
	    0deg,
	    rgba(161, 140, 209, 0.2) 0%,
	    rgba(251, 194, 235, 0.2) 100%
	  );
	}
	
	.container {
	  padding: 20rpx;
	}
	
	.header {
	  display: flex;
	  align-items: center;
	  justify-content: space-between;
	}
	
	.header>view {
	  width: calc((100% - 25rpx) / 2);
	  height: 100rpx;
	  border-radius: 20rpx;
	  background: rgba(255, 255, 255, 0.52);
	  box-shadow: 0px 0px 10px -20px rgba(0, 0, 0, 0.05);
	  display: flex;
	  align-items: center;
	  justify-content: center;
	  font-size: 15px;
	  color: #3b3b3b;
	}
	
	.score-type {
	  width: calc((100% - 20rpx) / 2);
	  height: 70rpx;
	  border-radius: 20rpx;
	  background: rgba(230, 230, 230, 0.8);
	  box-shadow: 0px 0px 20px -20px rgba(0, 0, 0, 0.05);
	  display: flex;
	  align-items: center;
	  justify-content: center;
	}
	
	.score-types {
	  padding: 10rpx 10rpx;
	  display: flex;
	  align-items: center;
	  justify-content: space-between;
	}
	
	.active {
	  background: #80de82; /* Light green color */
	  color: #ffffffff; /* White color */
	}
	
	.score-term {
	  padding: 8rpx 10rpx;
	  display: flex;
	  align-items: center;
	  justify-content: space-between;
	  font-size: 22rpx;
	  color: #3b3b3b;
	}
	
	.score-list {
	  width: 100%;
	  border-radius: 20rpx;
	  background: rgba(230, 230, 230, 0.8);
	  box-shadow: 0px 0px 10px -20px rgba(0, 0, 0, 0.05);
	  margin-top: 26rpx;
	  padding: 10rpx;
	}
	
	.term-name {
	  font-size: 32px;
	  color: #5555555;
	  text-align: center;
	}
	
	.list {
	  margin-top: 30rpx;
	}
	
	.score-item {
	  display: flex;
	  align-items: center;
	  justify-content: space-between;
	  min-height: 70rpx;
	  margin-top: 12rpx;
	}
	
	.score-item:not(:last-child) {
	  border-bottom: 1rpx solid rgba(232, 224, 235, 0.5);
	}
	
	.course-name {
	  font-size: 32px;
	  color: #3333333;
	}
	
	.course-score {
	  font-size: 32rpx;
	  color: #80de82; /* Light green color */
	}
	
	.fail {
	  color: #9c9c9c;
	}
	
	.raw-score_content {
	  font-weight: 400;
	  font-size: 24rpx;
	  color: #999999;
	}
</style -->>
