<!-- <template>
	<view class="list" v-if="list.length > 0">
		<view @click="goDetail(item.id)" class="item" v-for="(item, index) in list" :key="index" hover-class="none">
			<view class="info">
				<view class="text">
					<view class="title">课程名称:{{item.name}}</view>
					<view class="other">
						<view class="left">
							<view class="time">学期:{{item.term}}</view>
						</view>
						<view class="right">
							<view class="time">任课老师:{{item.tname}}</view>
						</view>
					</view>
				</view>
			</view>
			<view class="line"></view>
		</view>
		
	</view>
	
</template>
 
<script>
export default {
	name: 'courseList',
	props: {
		list: {
			type: Array,
			default: function(e) {
				return [];
			}
		}
	},
	data() {
		return {
		}
	},
	methods:{
		goDetail(id){
			uni.navigateTo({
				url: '/pages/index/detail?id='+id
			})
		}
	}
}
</script>
 
<style scoped lang="scss">
/*列表*/
.list {
	margin-top: 2rpx;
	.item {
		padding: 40rpx 24rpx 0 24rpx;
		.info {
			display: flex;
			flex-direction: row;
			justify-content: space-between;
			padding-bottom: 4rpx;
			.text {
				flex-grow: 1;
				flex-shrink: 1;
				display: flex;
				flex-direction: column;
				margin-right: 40rpx;
				.title {
					flex-grow: 1;
					flex-shrink: 1;
					font-size: 34rpx;
					line-height: 1.5;
					display: -webkit-box;
					text-overflow: ellipsis;
					word-break: break-all;
					-webkit-line-clamp: 2;
					-webkit-box-orient: vertical;
					overflow: hidden;
					height: 72rpx;
					margin-bottom: 8rpx;
				}
				.other {
					flex-grow: 1;
					flex-shrink: 1;
					display: flex;
					align-items: center;
					font-size: 28rpx;
					color: #999;
					line-height: normal;
					.left {
						display: flex;
						flex-grow: 1;
						flex-shrink: 1;
						.source {
							display: -webkit-box;
							text-overflow: ellipsis;
							word-break: break-all;
							-webkit-line-clamp: 1;
							-webkit-box-orient: vertical;
							overflow: hidden;
							width: 140rpx;
							margin-right: 16rpx;
						}
					}
					.right {
						display: flex;
						flex-direction: row;
						justify-content: center;
						align-items: center;
						flex-grow: 0;
						flex-shrink: 0;
						margin-right: 5rpx;
						image {
							flex-grow: 1;
							flex-shrink: 1;
							width: 28rpx;
							height: 28rpx;
							margin-right: 12rpx;
						}
						text {
							flex-grow: 1;
							flex-shrink: 1;
							margin-top: -6rpx;
						}
					}
				}
			}
			.photo {
				image {
					height: 170rpx;
					width: 222rpx;
					border-radius: 10rpx;
				}
			}
		}
		.line {
			display: bock;
			width: 100%;
			height: 1rpx;
			margin-top: 22rpx;
			background: #e8e8e8;
		}
		&:last-child {
			.line {
				//display: none;
			}
			//padding-bottom: 30rpx;
		}
	}
}
</style> -->
<template>
  <div class="container">
    <div class="header">课程表</div>
    <div class="table">
      <div class="row">
        <div class="cell time">课节</div>
        <div class="cell day">星期一</div>
        <div class="cell day">星期二</div>
        <div class="cell day">星期三</div>
        <div class="cell day">星期四</div>
        <div class="cell day">星期五</div>
        <div class="cell day">星期六</div>
        <div class="cell day">星期日</div>
      </div>
      <!-- 添加课程表数据 -->
      <div class="row" v-for="(item, index) in courseData" :key="index">
        <div class="cell time">{{ item.time }}</div>
        <div
          v-for="(day, dayIndex) in item.days"
          class="cell day"
          :key="dayIndex"
          :style="getCellStyle(day)"
        >
          {{ day }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      courseData: [
        {
          time: "1-2",
          days: ["军事理论", "", "", "大学物理A1", "高等数学A2", "", ""],
        },
        {
          time: "3-4",
          days: ["中国现代史纲要", "大学物理A1", "高等数学A2", "体育2", "大学外语2", "", ""],
        },
        {
          time: "5-6",
          days: ["高等数学A2", "大学外语2", "线性代数A", "面向对象程序设计", "形势与政策2", "", ""],
        },
        {
          time: "7-8",
          days: [
            "面向对象程序设计",
            "计算机组装与维护",
            "大学生职业生涯与发展规划",
            "Web前端开发技术",
            "线性代数A",
            "",
            "",
          ],
        },
        {
          time: "9-10",
          days: ["", "音乐鉴赏", "影视鉴赏", "", "", "", ""],
        },
        {
          time: "11-12",
          days: ["", "", "", "", "", "", ""],
        },
        {
          time: "13-14",
          days: ["", "", "", "", "", "", ""],
        },
        // 添加其他时间段的数据
      ],
    };
  },
  methods: {
    getCellStyle(day) {
      switch (day) {
        case "高等数学A2":
          return { background: "#658f69" };
        case "大学物理A1":
          return { background: "#658f61" };
        case "面向对象程序设计":
          return { background: "#658f66" };
        case "军事理论":
          return { background: "#658f6c" };
        case "大学外语2":
          return { background: "#a6c090" };
        default:
          return { background: "white" };
      }
    },
  },
};
</script>

<style>
 .container { 
	   padding: 20rpx;
	   background-color: #f7f7f7;
	   border-radius: 10rpx;
	   box-shadow: 0px 2px 6px rgba(0, 0, 0, 0.1); 
		}

.header { 
	font-size: 36rpx; 
	font-weight: bold; 
	text-align: center; 
	margin-bottom: 20rpx; 
	color: #333; 
	background-color: #eee; 
	padding: 10rpx; 
	border-radius: 5rpx; }

.table { 
	display: flex; 
	flex-direction: column; 
	margin-top: 20rpx; 
	}

.row { display: flex; }

.cell { 
	flex: 1; 
	text-align: center; 
	border: none; 
	padding: 10rpx; 
	background-color: #fff; 
	border-radius: 5rpx; 
	box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1); 
	margin: 5rpx; }

.time { 
	font-weight: bold;
	 background-color: #eee; }

.day { background-color: #eee; }
</style>