<template>
	<view class="calender">
		<view class="header">
			<picker mode="selector" :range="yearPicker" value="yearPicker[yearIndex].value" :range-key="'name'" @change="changeYear">
				<view>{{yearPicker[yearIndex].name}}</view>
			</picker>
			<picker mode="selector" :range="monthPicker" value="monthPicker[monthIndex].value" :range-key="'name'" @change="changeMonth">
				<view>{{monthPicker[monthIndex].name}}</view>
			</picker>
		</view>
	
	</view>
</template>

<script>
	export default {
		data() {
			return {
				yearPicker: [],
				monthPicker: [],
				yearIndex: 5,
				monthIndex: 0,
			}
		},
		methods: {
			// 获取现在的时间
			getRecentYear: function () {
				const now = new Date();
				let thisYear = now.getFullYear();
				let thisMonth = now.getMonth() + 1;
				this.monthIndex = thisMonth - 1;
				let thisDate = now.getDate();
				let thisDay = now.getDay();
				let yearPicker = [];
				for (let i = 0; i < 11; i++) {
				    let obj = {};
				    if (i < 5) {
				        obj.value = thisYear - 5 + i;
				    } else if (i == 5) {
				        obj.value = thisYear;
				    } else {
				        obj.value = thisYear + i;
				    }
				    obj.name = obj.value + '年';
				    yearPicker.push(obj);
				}
				let monthPicker = [];
				for (let j = 1; j < 13; j++) {
				    let obj = {};
				    obj.value = j;
				    obj.name = j < 10 ? '0' + j + '月' : j + '月';
				    monthPicker.push(obj);
				}
				this.yearPicker = yearPicker;
				this.monthPicker = monthPicker;
			},
			// 是否为闰年
			leapYear: function (year) {
				if (year % 4 == 0 && year % 100 != 0 || year % 400 == 0) {
				    return true;
				} else {
				    return false;
				}
			},
			// 一个月有多少天
			daysInMonth: function (month, leap) {
				let days;
				switch (month) {
				    case 1:
				    case 3:
				    case 5:
				    case 7:
				    case 8:
				    case 10:
				    case 12:
				        days = 31;
				        break;
				    case 2:
				        if (leap) {
				            days = 29;
				        } else {
				            days = 28;
				        }
				        break;
				    case 4:
				    case 6:
				    case 9:
				    case 11:
				        days = 30;
				        break;
				}
				return days;
			},
			changeYear: function (e) {
				console.log(e);
				this.yearIndex = e.detail.value;
			},
			changeMonth: function (e) {
				console.log(e);
				this.monthIndex = e.detail.value;
			}
		},
		onLoad: function (options) {
			this.getRecentYear();
		}
	}
</script>

<style>

</style>
