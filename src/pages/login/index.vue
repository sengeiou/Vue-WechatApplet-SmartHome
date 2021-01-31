<template>
  <div class="bc">
    <div class="header">
      <div v-if="islogin">
        <div class="header-title">请登录</div>
        <div class="header-info">欢迎登陆我的智能家居</div>
      </div>
      <div v-else>
        <div class="header-title">请注册</div>
        <div class="header-info">注册账号登陆管理智能家居</div>
      </div>
    </div>
    <div class="body">
      <div class="login-form">
        <van-field placeholder="请输入用户名" :value="inputUserName" @change="onUserNameChange" />
        <van-field placeholder="请输入密码" :value="inputPassword" @change="onPasswordChange" type="password"/>
        <div v-if="!islogin">
          <van-field placeholder="请输入手机号" :value="inputContect" @change="onContectChange" />
        </div>
      </div>
      <van-button slot="button" round block color="#F08080" @click="onClick">{{islogin?"登录":"注册"}}</van-button>
      <div class="other-option">
        <div @click="onOptionClick">
          <span>{{islogin?"注册账号":"登录账号"}}</span>
        </div>
        <span style="margin:0 30px">|</span>
        <div @click="onForgetClick">
          <span>忘记密码</span>
        </div>
      </div>
      <div class="copyright-wrapper">
        <span class="copyright">Power By Lixin</span>
      </div>
      <van-dialog
        use-slot
        title="找回密码效验"
        :show="showFindPW"
        show-cancel-button
        transition="fade"
        @confirm="onFindPWConfirm"
        @cancle="onFindPWCancle"
      >
      <van-field label="手机号" title-width="60px" placeholder="请输入手机号" :value="inputContect" @change="onContectChange" />
      </van-dialog>
      <van-dialog
        use-slot
        title="重置密码"
        :show="showResetPW"
        show-cancel-button
        transition="fade"
        @confirm="onResetPWConfirm"
        @cancle="onResetPWCancle"
      >
      <van-field label="用户名" title-width="60px" :value="inputUserName" />
      <van-field label="新密码" title-width="60px" placeholder="请输入新的密码" :value="inputPassword" type="password" @change="onPassWordChange"/>
      </van-dialog>
      <van-toast id="van-toast" />
    </div>
  </div>
</template>

<script>
import Toast from "vant-weapp/dist/toast/toast"

export default {
  data () {
    return {
      inputUserName:'',
      inputPassword:'',
      inputContect:'',
      islogin:true,
      showFindPW:false,
      showResetPW:false,
    }
  },
  methods:{
    onUserNameChange(event){
      var that = this
      that.$set(that, "inputUserName",event.mp.detail)
      console.log("当前输入的用户名:=",event.mp.detail)
    },
    onPasswordChange(event){
      var that = this
      that.$set(that, "inputPassword",event.mp.detail)
      console.log("当前输入的密码:=",event.mp.detail)
    },
    onContectChange(event){
      var that = this
      that.$set(that, "inputContect",event.mp.detail)
      console.log("当前输入的手机号:=",event.mp.detail)
    },
    //登录注册按钮点击事件
    onClick(event){
      var that = this
      Toast.loading({//提示弹窗
        duration:0,//持续展示弹窗
        forbidClick :true,//禁止背景被点击
        message:that.islogin?'登陆中...':'注册中...'
      }),
      //模拟请求服务器时候的延时
      setTimeout(()=>{
        if(!that.islogin){//在注册页面
          // wx.getStorage({
          //   key:"user",
          //   success(res){
          //     //判断用户名是否存在
          //     if(that.inputUserName === res.data.username||that.inputContect===res.data.contect){
          //       Toast.fail("该用户名或手机号已注册");
          //       that.inputUserName='';//清空用户名输入框
          //       that.inputContect= '';//清空手机输入框
          //     }
          //     else{
          //       //判断输入框是否为空
          //       if(that.inputUserName===''||that.inputPassword===''||that.inputContect===''){
          //           Toast.fail("请输入用户名，密码，手机号")
          //         }
          //       else{
          //         wx.setStorage({
          //           key:"user",
          //           data:{
          //             username:that.inputUserName,
          //             password:that.inputPassword,
          //             contect:that.inputContect,
          //           },
          //           success (res) {
          //             console.log(res)
          //             console.log("注册成功");
          //             Toast.success("注册成功")
          //             that.islogin = !that.islogin//注册成功后跳转到登录界面
          //             that.inputContect=''//注册完以后清空手机号
          //           },
          //           fail (res) {
          //             console.log(res)
          //             console.log("注册失败");
          //             Toast.fail("注册失败")
          //           }
          //         });
          //       }
          //     }
          //   }
          // })
           //判断输入框是否为空
                if(that.inputUserName===''||that.inputPassword===''||that.inputContect===''){
                    Toast.fail("请输入用户名，密码，手机号")
                  }
                else{
                  wx.setStorage({
                    key:"user",
                    data:{
                      username:that.inputUserName,
                      password:that.inputPassword,
                      contect:that.inputContect,
                    },
                    success (res) {
                      console.log(res)
                      console.log("注册成功");
                      Toast.success("注册成功")
                      that.islogin = !that.islogin//注册成功后跳转到登录界面
                      that.inputContect=''//注册完以后清空手机号
                    },
                    fail (res) {
                      console.log(res)
                      console.log("注册失败");
                      Toast.fail("注册失败")
                    }
                  });
                }
        }
        else{//登录界面
          console.log("进入登录界面");
          //判断输入框是否为空
          if(that.inputUserName===''||that.inputPassword===''){
            Toast.fail("用户名，密码不能为空")
          }
          else{
            wx.getStorage({
            key:"user",
            success (res) {
              console.log(res)
              //判断用户名和密码是否存在
              if(that.inputUserName === res.data.username &&
                that.inputPassword === res.data.password){
                Toast.success("登录成功")
                that.inputUserName='';//清空用户名输入框
                that.inputPassword='';//清空密码输入框
                //延时过后跳转到首页
                setTimeout(()=>{
                  wx.switchTab({
                    url: '/pages/index/main'
                  },200)
                })
              }
              else{
                Toast.fail("用户名或密码不正确")
              }
            },
            fail (res) {
              console.log(res)
              Toast.success("数据库中没有数据")
            }
          });
          }
        }
      },1000)
    },
    //注册和登录切换的点击事件
    onOptionClick(event){
      let that = this
      that.islogin = !that.islogin
      // 在切换页面的时候清空输入框的所有信息
      that.inputUserName='';
      that.inputPassword='';
      that.inputContect='';
      console.log(`切换登陆/注册按钮被点击了当前处于${that.islogin?"登陆":"注册"}页面`);
    },
    //忘记密码点击事件函数
    onForgetClick(event){
      let that = this;
      that.showFindPW=true;
    },
    //找回密码弹框确定按键的函数
    onFindPWConfirm(event){
      let that = this;
      that.showFindPW = false;//改变找回密码弹框的参数show的布尔值
        wx.getStorage({
            key:"user",
            success (res) {
              console.log(res)
              //判断手机号是否存在
              if(that.inputContect === res.data.contect){
                that.inputUserName=res.data.username; //把缓存数据里面的用户名添加到找回密码弹框的输入框 该输入框设置了只读属性readonly
                that.showResetPW=true;
                //console.log("找到用户",res.data.username);
              }
              else{
                Toast.fail("无该用户信息");
              }
            },
            fail (res) {
              console.log(res);
              Toast.success("数据库中没有数据");
            }
        });
    },
    //找回密码弹框取消按钮的函数
    onFindPWCancle(){
      let that = that;
      that.showFindPW =false;
      that.inputContect='';//清除手机号
    },
    //重置密码弹框确定按键的函数
    onResetPWConfirm(envent){
      let that = this
      that.showFindPW= false;
      that.showResetPW=false;
      wx.setStorage({
          key:"user",
          data:{
            username:that.inputUserName,
            password:that.inputPassword,
            contect:that.inputContect,
          },
            success (res) {
              console.log(res)
              console.log("密码重置成功");
              Toast.success("密码重置成功")
            },
            fail (res) {
              console.log(res)
              console.log("密码重置失败");
              Toast.fail("密码重置失败");
            }
        });
    },
    //重置密码弹框取消按键的函数
    onResetPWCancle(){
      let that = this;
      that.showFindPW = false;
      that.showResetPW =false;
      that.inputUserName='';//清空用户输入框
      that.inputPassword='';//清空密码输入框
      that.inputContect='';//清空手机号输入框
    }
  },
}
</script>

<style lang="scss" scoped>
.bc{
  background: radial-gradient(200% 100% at bottom center, #f7f7b6, #e96f92, #1b2947);
  background: radial-gradient(220% 105% at top center, #1b2947 10%, #75517d 40%, #e96f92 65%, #f7f7b6);
  .header {
  height: 100px;
  padding: 30px 0;
  color: #fff;
  .header-title {
    font-size: 28px;
    font-weight: 500;
    margin-left: 40px;
    margin-top:10px ;
  }
  .header-info {
    font-size: 16px;
    margin-left: 40px;
    margin-top: 10px ;
  }
  }
  .body {
    padding: 20px;
    //  margin-bottom: 90%;
    .login-form {
      margin-bottom: 30px;
    }
    .other-option {
      display: flex;
      justify-content: center;
      margin-top: 20px;
      color: #4d4b4b;
      margin-bottom: 100%;
    }
    .copyright-wrapper {
      display: flex;
      justify-content: center;
      .copyright {
        color: rgb(114, 112, 112);
        position: fixed;
        bottom: 10%;
      }
    }
  }
}
</style>
