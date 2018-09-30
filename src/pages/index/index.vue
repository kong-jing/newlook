<template>
  <div class="weather-wrapper">
    <!-- 地点 -->
    <div class="location-wrapper">
      <div class="location">
        <img class="location-icon" src="/static/images/location-icon.png" >
        <div class="location-text">{{city}}</div>
      </div>
      <div class="location-tips" v-if="locationAuthType==0">点击获取当前位置</div>
      <div class="location-tips" v-if="locationAuthType==1">点击打开位置权限</div>
      <div class="location-tips" v-if="locationAuthType==2"></div>
    </div>
  </div>
</template>

<script>
import card from '@/components/card'

let UNPROMPTED = 0
let UNAUTHORIZED = 1
let AUTHORIZED = 2

export default {
  data () {
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
    onPullDownRefresh: function(){  //下拉刷新的回调
      this.getNow(() => {
        wx.stopPullDownRefresh();
      });
    },
    getNow(callback){
      wx.request({
        url: 'https://test-miniprogram.com/api/weather/now',
        data: {
          city: this.data.city
        },
        header: {
          'content-type': 'application/json'  //默认值
        },
        success: res => {
          console.log(res.data);
        }

      })
    }
  },

  created () {
    // 调用应用实例的方法获取全局数据
  }
}
</script>

<style lang="scss" scoped>
.location-icon{
  margin-right: 10rpx;
  width: 21rpx;
  height: 30rpx;
}
</style>
