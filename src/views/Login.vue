<template>
  <div class="login">
    <van-nav-bar left-text="返回" right-text="先逛一逛" @click-right="goState('Home')">
      <template #left>
        <div class="nav-left">
          <div class="logo">
            <img
              class="auto-img"
              src="../assets/images/home_active.png"
              alt=""
            />
          </div>
          <div class="logo-text">Tiki咖啡</div>
        </div>
      </template>
    </van-nav-bar>

    <div class="user-box">
      <div class="user-title">
        <div class="welcome">欢迎回来!</div>
        <!-- <div class="sub-title">Please login to your accounts</div> -->
      </div>

      <van-form>
        <van-field
          v-model="userInfo.phone"
          name="手机号"
          label="手机号"
          placeholder="手机号"
          autocomplete="off"
          @click="show = true"
        />

        <van-number-keyboard
          v-model="userInfo.phone"
          :show="show"
          @blur="show = false"
        />

        <van-field
          v-model="userInfo.password"
          :type="isPassword ? 'password' : 'text'"
          name="密码"
          label="密码"
          placeholder="密码必须为字母开头"
          :right-icon="isPassword ? 'closed-eye' : 'eye-o'"
          autocomplete="off"
          @click-right-icon="toggleisPasswordType"
        />
        <div class="forgot">
          <span @click="goState('Forgot')">忘记密码?</span>
        </div>

        <div class="commit-btn">
          <van-button round block color="skyblue" @click="login">
            登录
          </van-button>
        </div>
        <div class="commit-btn">
          <van-button round block type="default" @click="toggleRegisterBox">
            注册
          </van-button>
        </div>
      </van-form>
    </div>

    <!-- 注册 -->
    <van-popup
      class="register-box"
      v-model="isShow"
      position="bottom"
      closeable
    >
      <div class="register-title">注册</div>
      <van-form>
        <van-field
          v-model="userRegisterInfo.phone"
          name="手机号"
          label="手机号"
          placeholder="11位手机号"
          autocomplete="off"
        />

        <van-field
          v-model="userRegisterInfo.password"
          :type="isRegisterPassword ? 'password' : 'text'"
          name="密码"
          label="密码"
          placeholder="密码必须为字母开头"
          :right-icon="isRegisterPassword ? 'closed-eye' : 'eye-o'"
          autocomplete="off"
          @click-right-icon="toggleRegsiterPasswordType"
        />

        <van-field
          v-model="userRegisterInfo.nickName"
          name="昵称"
          label="昵称"
          placeholder="昵称"
          autocomplete="off"
        />

        <div class="commit-btn register-btn">
          <van-button round block color="skyblue" @click="register">
            注册
          </van-button>
        </div>
      </van-form>
    </van-popup>
  </div>
</template>

<script>
//导入外部样式表
import "../assets/less/login.less";

//导入表单验证模块
import { validForm } from "../assets/js/validForm";

export default {
  name: "Login",
  data() {
    return {
      show: false,
      //用户登录信息
      userInfo: {
        phone: "",
        password: "",
      },

      //用户注册信息
      userRegisterInfo: {
        phone: "",
        password: "",
        nickName: "",
      },

      // 切换登录密码框的类型
      isPassword: true,

      //是否显示注册框
      isShow: false,

      //切换注册密码框的类型
      isRegisterPassword: true,
    };
  },

  methods: {
    //切换注册框
    toggleRegisterBox() {
      this.isShow = true;
    },

    // 切换登陆密码框的数据类型
    toggleisPasswordType() {
      this.isPassword = !this.isPassword;
    },

    //切换注册密码框的类型
    toggleRegsiterPasswordType() {
      this.isRegisterPassword = !this.isRegisterPassword;
    },

    //注册
    register() {
      //构造表单验证信息
      let o = {
        phone: {
          value: this.userRegisterInfo.phone,
          errorMsg: "手机号格式不正确",
          reg: /^1[2-9]\d{9}$/,
        },
        password: {
          value: this.userRegisterInfo.password,
          errorMsg: "密码格式不正确",
          reg: /^[A-Za-z]\w{5,15}$/,
        },
        nickName: {
          value: this.userRegisterInfo.nickName,
          errorMsg: "昵称格式不正确",
          reg: /^[\w\u4e00-\u9fa5]{1,10}$/,
        },
      };

      let isPass = validForm.valid(o);

      if (isPass) {
        // 

        //赋值用户注册信息
        let userInfo = Object.assign({}, this.userRegisterInfo);
        userInfo.appkey = this.appkey;

        // 
        // 

        // 启动加载提示
        this.$toast.loading({
          // 文本提示
          message: "加载中...",
          // 禁止穿透点击
          forbidClick: true,
          // 提示时间  没有时间限制  需要手动关闭
          duration: 0,
        });

        //发起注册请求
        this.axios({
          //请求类型
          method: "POST",
          //请求路径
          url: "/register",

          //POST请求参数, object
          data: userInfo,
        })
          .then((result) => {
            //关闭加载提示
            this.$toast.clear();

            // 判断
            if (result.data.code == 100) {
              this.isShow = false;
            } else {
              // 如果注册失败 则显示失败文案
              this.$toast("提示文案");
            }

            
          })
          .catch((err) => {
            this.$toast.clear();

            
          });
      }
    },

    // 登录
    login() {
      let o = {
        phone: {
          value: this.userRegisterInfo.phone,
          errorMsg: "手机号格式不正确",
          reg: /^1[2-9]\d{9}$/,
        },
        password: {
          value: this.userRegisterInfo.password,
          errorMsg: "密码格式不正确",
          reg: /^[A-Za-z]\w{5,15}$/,
        },
      };

      let isPass = validForm.valid(o);
      if (isPass) {
        //发起登录请求
        //复制用户注册信息
        let userInfo = Object.assign({}, this.userInfo);
        userInfo.appkey = this.appkey;

        this.$toast.loading({
          // 文本提示
          message: "加载中...",
          // 禁止穿透点击
          forbidClick: true,
          // 提示时间  没有时间限制  需要手动关闭
          duration: 0,
        });

        //发起注册请求
        this.axios({
          //请求类型
          method: "POST",
          //请求路径
          url: "/login",

          //POST请求参数, object
          data: userInfo,
        })
          .then((result) => {
            //关闭加载提示
            this.$toast.clear();

            
            // 判断
            if (result.data.code == 200) {
              // 登陆成功
              // 保存token，以便后面使用验证
              localStorage.setItem("_tk", result.data.token);
              //其他操作
              this.$router.push({ name: "Home" });
            } else {
              this.$toast(result.data.msg);
            }
          })
          .catch((err) => {
            this.$toast.clear();

            
          });
      }
    },

    //跳转页面
    goState(name) {
      this.$router.push({ name });
    },
  },
};
</script>