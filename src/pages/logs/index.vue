<template>
  <div class="body">
    <div class="information">
      <div class="tou">
        <img class="logo" src="/static/images/user.jpg" />
      </div>
      <div class="user">用户: {{username}}</div>
    </div>
    <div class="btn">
      <van-button slot="button" round block color="#95EC69" @click="changeClick" >切换用户</van-button>
    </div>
  </div>
</template>

<script>

export default {

  data () {
    return {
      username:'',
    }
  },
  methods: {
    changeClick(evnt){
      wx.navigateTo({
        url: '/pages/login/main'
      },200)
    }
  },
  onShow(){
    let that = this
    wx.getStorage({
      key:"user",
      success (res) {
        // console.log(res.data);
        that.username=res.data.username
      }
    });
  }
}
</script>

<style  lang="scss" scoped >
.body{
  padding: 30px;
  .information{
    // background:url(https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1600615777428&di=160306444529db7a7382ccecd3da55cc&imgtype=0&src=http%3A%2F%2Fimg.pconline.com.cn%2Fimages%2Fupload%2Fupc%2Ftx%2Fwallpaper%2F1212%2F06%2Fc0%2F16390940_1354774093291.jpg);
    // background-size:100% 100%;
    background: linear-gradient(to bottom,#03beec,#63da53);
    border-radius: 15px;
    padding: 20px;
    .tou{
      height: 60px;
      width: 60px;
      margin: 0 auto;
      border-radius: 15%;
      background-color: #fff;
      .logo{
        height: 100%;
        width: 100%;
      }
    }
    .user{
      color: #fff;
      font-size: 20px;
      margin: 30px auto;
      text-align: center;
    }
  }
  .btn{
  margin-top: 50%;
  }
}


</style>
