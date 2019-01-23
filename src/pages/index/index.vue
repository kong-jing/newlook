<template>
<div>
  <div class="weather-wrapper">
    <!-- 地点 -->
    <div class="location-wrapper">
      <div class="location">
        <img class="location-icon" src="/static/images/location-icon.png">
        <div class="location-text">{{city}}</div>
      </div>
      <div class="location-tips" v-if="locationAuthType==0">点击获取当前位置</div>
      <div class="location-tips" v-if="locationAuthType==1">点击打开位置权限</div>
      <div class="location-tips" v-if="locationAuthType==2"></div>
    </div>
    <div class="temp">{{nowTemp}}</div>
    <div class="weather">{{nowWeather}}</div>
    <img class="weather-bg" :src=nowWeatherBackground />
    <!-- 添加新元素 -->
    <div class="day-weather" @click="onTapDayWeather">
      <div class="day-text">{{todayDate}}</div>
      <div class="temp-text">{{todayTemp}}</div>
      <img class="arrow-icon" src="/static/images/arrow.png" />
    </div>
  </div>
  <div class="timetips">
    <img class="timetips-icon" src="/static/images/time-icon.png"/>
    <div class="timetips-text">未来24小时天气预测</div>
  </div>
  <!-- 侧滑列表 -->
</div>
</template>

<script>
  import card from '@/components/card'

  let UNPROMPTED = 0
  let UNAUTHORIZED = 1
  let AUTHORIZED = 2

  export default {
    data() {
      return {
        city: '重庆',
        nowTemp: 14,
        nowWeather: '多云',
        nowWeatherBackground: '',
        hourlyWeather: [],
        todayTemp: "",
        todayDate: "",
        locationAuthType: UNPROMPTED
      }
    },

    components: {
      card
    },

    methods: {
      onPullDownRefresh: function () { //下拉刷新的回调
        this.getNow(() => {
          wx.stopPullDownRefresh();
        });
      },
      getNow(callback) {
        wx.request({
          url: 'https://test-miniprogram.com/api/weather/now',
          data: {
            city: this.data.city
          },
          header: {
            'content-type': 'application/json' //默认值
          },
          success: res => {
            console.log(res.data);
            let result = res.data.result;
            this.setNow(result);
            this.setHourlyWeather(result);
            //设置forecast参数
            this.setToday(result);
          },
          fail: res => {
            wx.stopPullDownRefresh();
          }, 
          complete: () => {
            callback && callback();
          }

        })
      }
    },

    onShow() {
      this.qqmapsdk = new QQMapWX({
        key: 'BRXBZ-QRJCX-T2543-7DL7T-P72IO-5EBBF'
      });
      
      wx.getSetting({
        success: res => {
          let auth = res.authSetting['scope.userLocation']
          console.log(auth)
          this.setData
        }
      })
    },

    created() {
      // 调用应用实例的方法获取全局数据
    }
  }

</script>

<style lang="scss" scoped>
  .location-icon {
    margin-right: 10rpx;
    width: 21rpx;
    height: 30rpx;
  }
 
/*地址  */
.location-wrapper{
  margin-bottom: 60rpx;
  font-size: 30rpx;
  line-height: 42rpx;
  color: rgba(0, 0, 0, 0.5);
}
.location{
  display: flex;
  justify-content: center;
  align-items: center;
}
.location-icon{
  margin-right: 10rpx;
  width: 21rpx;
  height: 30rpx;
}
.location-text{
  opacity: 0.5;
  line-height: 42rpx;
}
.location-tips{
  text-align: center;
  opacity: 0.5;
  margin-top: 10rpx;
  line-height: 42rpx;
  height: 42rpx;
}
/*温度  */
.temp{
  margin-top: 200rpx;
  text-align: center;
  font-size: 200rpx;
  line-height: 280rpx;
  opacity: 0.8;
}

.weather{
  text-align: center;
  font-size: 40rpx;
  line-height: 56rpx;
  opacity: 0.85;
}

.weather-wrapper{
  position: relative;
  padding-top: 20rpx;
  padding-bottom: 200rpx;
  /* background-color: moccasin; */
}

.weather-bg{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
}

.timetips{
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20rpx;
  margin-bottom: 20rpx;
}

.timetips-icon{
  margin-right: 10rpx;
  width: 36rpx;
  height: 28rpx;
}

.timetips-text{
  font-size: 30rpx;
  line-height: 42rpx;
  opacity: 0.5;
}
.forecast-list{
  display: flex
}

.forecast-item{
  display: flex;
  flex-direction: column;
  width: 120rpx;
  flex-shrink: 0;
  align-items: center;
}

.forecast-time{
  font-size: 30rpx;
  line-height: 42rpx;
}

.forecast-weather{
  margin-top: 50rpx;
  margin-bottom: 50rpx;
  width: 60rpx;
  height: 60rpx;
}

.forecast-temp{
  font-size: 40rpx;
  line-height: 56rpx;
}

.day-weather{
  display: flex;
  align-items: center;
  position: absolute;
  bottom: 0;
  left: 40rpx;
  right: 40rpx;
  height: 90rpx;
  border-top: 1px solid rgba(0, 0, 0, 0.1);
  font-size: 30rpx;
  line-height: 42rpx;
  opacity: 0.5;
}

.temp-text{
  flex-grow: 1;
  padding-right: 30rpx;
  text-align: right;
}

.arrow-icon{
  width: 13rpx;
  height: 24rpx;
}

</style>
