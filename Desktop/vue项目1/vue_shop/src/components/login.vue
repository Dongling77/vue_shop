<template>
  <div class="conter">
    <div class="login_box">
      <div class="avatar_box">
        <img src="../assets/logo.png" alt />
      </div>
      <el-form :model="loginform" :rules="loginrules"
      ref="loginformref" 
      label-width="0px" class="el_form">
        
        <el-form-item prop="username">
          <el-input
            v-model="loginform.username"
            prefix-icon="el-icon-user-solid"
            
            placeholder="输入用户名"
          >
            
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            v-model="loginform.password"
            prefix-icon="el-icon-lock"
            type="password"
            placeholder="输入密码"
          >
            <i slot="suffix" class="el-input__icon el-icon-view"></i>
          </el-input>
        </el-form-item>

        <el-form-item class="emm">
          <el-button type="primary" @click="login">登录</el-button>

          <el-button type="info" @click="resetform">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: "login",
  data() {
    return {
      loginform: {
        username: "",
        password: ""
      },
      loginrules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          { min: 3, max: 10, message: "长度在 3 到 10 个字符", trigger: "blur" }
        ],
        password:[
             { required: true, message: "请输入用户名", trigger: "blur" },
          { min: 3, max: 10, message: "长度在 3 到 10 个字符", trigger: "blur" }
        ]
      }
    };
  },
  methods:{
resetform(){
    console.log(this);
    this.$refs.loginformref.resetFields();
},
login(){
    this.$refs.loginformref.validate(async valid=> {
        if(!valid) return;
        const {data:res} =await  this.$http.post("login",this.loginform);
        // console.log(res);
        if(res.meta.status !==200) return this.$message.error("登陆失败");
        this.$message.success('你这瓜多少钱一斤');
        //登录成功之后的 token,保存到客户端的 sessionStorage中
        //  1.1 项目中除了登录之外的其他API接口 必须在登录之后才能访问
        // 1.2 token 只应当在当前网站打开期间生效 所以讲 token 保存在sessionStorage中
        window.sessionStorage.setItem("token",res.data.token);
        //2通过编程式导航跳转到后台主页，路由地址是 /home
        this.$router.push("/home");
    });
}
  }
};
</script>

<style  scoped>
.conter {
  background-color: #2ff4f6;
  height: 100%;
}
.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 0 10px #ddd;
}
.avatar_box {
  height: 130px;
  width: 130px;
  background-color: #fff;
  border: 1px solid #fff;
  overflow: hidden;
  border-radius: 50%;

  position: relative;
  left: 50%;
  transform: translate(-50%, -50%);
}
.avatar_box > img {
  height: 130px;
  width: 130px;
}
.el_form {
  width: 90%;
  padding-left: 20px;
}
.emm {
  display: flex;
  justify-content: right;
}
</style>

