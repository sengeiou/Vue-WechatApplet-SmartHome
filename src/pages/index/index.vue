<template>
  <div class="wrapper">
    <div class="header-wrapper">
      <div class="header-title">
        <span>空气质量-{{airText}}</span>
        <span>{{city}}</span>
      </div>
      <div class="header-text">
        <span>{{win}}:{{win_speed}}</span>
        <span>{{weather}}</span>
      </div>
      <div class="weather-advice">{{weatherAdvice}}</div>
    </div>
    <div class="body-wrapper">
      <div class="body">
        <div class="data-wrapper">
          <div class="data">
            <img class="data-logo" src="/static/images/wendu.png" />
            <div class="data-text">
              <div class="data-title">温度</div>
              <div class="data-value">{{tem}}℃</div>
            </div>
          </div>
          <div class="data">
            <img class="data-logo" src="/static/images/shidu.png" />
            <div class="data-text">
              <div class="data-title">湿度</div>
              <div class="data-value">{{hum}}%</div>
            </div>
          </div>
        </div>
        <div class="data-wrapper">
          <div class="data">
            <img class="data-logo" src="/static/images/guang.png" />
            <div class="data-text">
              <div class="data-title">光照度</div>
              <div class="data-value">{{lux}}lux</div>
            </div>
          </div>
          <div class="data">
            <img class="data-logo" src="/static/images/wled.png" />
            <div class="data-text">
              <div class="data-title">客厅灯</div>
              <div class="data-value">
                <switch @change="kledchange" :checked="kled" color="#95EC69" />
              </div>
            </div>
          </div>
        </div>
        <div class="data-wrapper">
          <div class="data">
            <img class="data-logo" src="/static/images/woled.png" />
            <div class="data-text">
              <div class="data-title">卧室灯</div>
              <div class="data-value">
                <switch @change="bledchange" :checked="bled" color="#95EC69" />
              </div>
            </div>
          </div>
          <div class="data">
            <img class="data-logo" src="/static/images/feng.png" />
            <div class="data-text">
              <div class="data-title">风扇</div>
              <div class="data-value">
                <switch @change="fledchange" :checked="fled" color="#95EC69" />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>




export default {
  data () {
    return {
      area:'请求中',//城区
      city:'请求中',//城市
      airText: '请求中',//空气优良
      airValue:'',//空气指数
      weather:'请求中',//天气
      weatherAdvice:'请求中',//天气建议
      win:'请求中',//风向
      win_speed:'请求中',//风力等级
      tem:'...',//温度
      hum:'...',//湿度
      lux:'...',//光照强度
    }
  },

  components: {
  },

  methods: {
    //控制客厅灯
    kledchange(event){
      let sw = event.mp.detail.value
      const devicesid ='624519850'//设备id
      const datastreams = 'temperature,humidity,illumination'//数据流ID
      const apikey = '2oc6=0XgNsKFW8XVsGcpSJmdlL0='//
      // console.log(sw);
      if(sw){
        wx.request({
          url: `http://api.heclouds.com/cmds?device_id=${devicesid}`,
          header: {
            'api-key': `${apikey}`
          },
          method: 'POST',
          data: 111,
          success(res){
            console.log(res.data)
          },
          fail(res){
            console.log(res)
          }
        })
      }
      else{
        wx.request({
          url: `http://api.heclouds.com/cmds?device_id=${devicesid}`,
          header: {
            'api-key': `${apikey}`
          },
          method: 'POST',
          data: 110,
          success(res){
            console.log(res.data)
          },
          fail(res){
            console.log(res)
          }
        })
      }
    },
    //控制卧室灯
    bledchange(event){
      let sw = event.mp.detail.value
      const devicesid ='624519850'//设备id
      const apikey = '2oc6=0XgNsKFW8XVsGcpSJmdlL0='//
      // console.log(sw);
      if(sw){
        wx.request({
          url: `http://api.heclouds.com/cmds?device_id=${devicesid}`,
          header: {
            'api-key': `${apikey}`
          },
          method: 'POST',
          data: 121,
          success(res){
            console.log(res.data)
          },
          fail(res){
            console.log(res)
          }
        })
      }
      else{
        wx.request({
          url: `http://api.heclouds.com/cmds?device_id=${devicesid}`,
          header: {
            'api-key': `${apikey}`
          },
          method: 'POST',
          data: 120,
          success(res){
            console.log(res.data)
          },
          fail(res){
            console.log(res)
          }
        })
      }
    },
    //控制风扇
    fledchange(event){
      let sw = event.mp.detail.value
      const devicesid ='624519850'//设备id
      const apikey = '2oc6=0XgNsKFW8XVsGcpSJmdlL0='//
      // console.log(sw);
      if(sw){
        wx.request({
          url: `http://api.heclouds.com/cmds?device_id=${devicesid}`,
          header: {
            'api-key': `${apikey}`
          },
          method: 'POST',
          data: 131,
          success(res){
            console.log(res.data)
          },
          fail(res){
            console.log(res)
          }
        })
      }
      else{
        wx.request({
          url: `http://api.heclouds.com/cmds?device_id=${devicesid}`,
          header: {
            'api-key': `${apikey}`
          },
          method: 'POST',
          data: 130,
          success(res){
            console.log(res.data)
          },
          fail(res){
            console.log(res)
          }
        })
      }
    },
  },

  created () {
    // let app = getApp()
  },
  onShow(){
    let that = this

    //获取天气信息
    wx.getLocation({
      type: "wgs84",
      success(res) {
        const latitude = res.latitude;
        const longitude = res.longitude;
        const mykey = '161cff18337a40f7a811b267a8727712';
        wx.request({
           //位置请求地址 这里用的是模板字符串``
          url:`https://geoapi.heweather.net/v2/city/lookup?location=${longitude},${latitude}&key=${mykey}`,//和风天气api
          success (res) {
            // console.log(res.data)
            const location = res.data.location[0]
            var id = location.id//通过和风天气api获取的城市编码
            // console.log(id);
            wx.request({
             //天气api请求地址 这里用的是模板字符串`` 武夷山城市编码101230905
            url:`https://v0.yiketianqi.com/api?version=v61&appid=31143983&appsecret=VisL2wEO&cityid=${id}`,//天气api
            success (res) {
              //console.log(res.data)
              //天气api获取的参数传递
              // const {now} = res.data
              that.city = res.data.city
              that.weatherAdvice=res.data.air_tips
              that.airText =res.data.air_level
              that.airValue = res.data.air_pm25
              that.weather =res.data.wea
              that.area= res.data.country
              that.win = res.data.win
              that.win_speed= res.data.win_speed
            }
            })
          }
        })
      }
    });

    const devicesid ='624519850'//设备id
    const datastreams = 'temperature,humidity,illumination'//数据流ID
    const apikey = '2oc6=0XgNsKFW8XVsGcpSJmdlL0='//

    //获取设备数据流
    wx.request({
      url: `https://api.heclouds.com/devices/${devicesid}/datastreams?datastream_ids=${datastreams}`,
      header: {
            "api-key": `${apikey}`,
          },
      success(res){
        console.log(res.data);
        const tem = res.data.data[0]
        const hum = res.data.data[1]
        const lux = res.data.data[2]

        that.tem = tem.current_value
        that.hum = hum.current_value
        that.lux = parseInt(lux.current_value)//由于光照值有小数所以这里需要转换成整数
      },
      fail(){
        wx.showToast({
          title: '与服务器通信失败',
          icon: 'fail',
          duration: 2000
        })
      }
    })
  },
}
</script>

<style lang="scss" scoped>
.wrapper {
  padding: 15px;
  .header-wrapper {
    background: linear-gradient(45deg,#feac5e,#c779d0,#4bc0c8);
    border-radius: 20px;
    color: #fff;
    box-shadow: #d6d6d6 0px 0px 5px;
    padding: 15px 30px;
    .header-title {
      display: flex;
      justify-content: space-between;
    }
    .header-text {
      margin-top: 5px;
      font-size: 22px;
      font-weight: 400;
      display: flex;
      justify-content: space-between;
    }
    .weather-advice {
      margin-top: 15px;
      font-size: 13px;
    }
  }
  .data-wrapper {
    margin-top: 20px;
    display: flex;
    justify-content: space-between;
    .data {
      background-color: #fff;
      width: 150px;
      height: 80px;
      border-radius: 20px;
      display: flex;
      justify-content: space-around;
      padding: 0 8px;
      box-shadow: #d6d6d6 0px 0px 5px;
      .data-logo {
        height: 45px;
        width: 40px;
        margin-top: 15px;
      }
      .data-text {
        margin-top: 10px;
        color: #7f7f7f;
        .data-value {
          font-size: 20px;
        }
      }
    }
  }
}
</style>
