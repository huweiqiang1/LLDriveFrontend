<template>
  <div class="login-body">
    <div class="bg"></div>
    <!-- 登录框 -->
    <div class="login-panel">
      <el-form class="login-register" :model="formData" :rules="rules" ref="formDataRef" @submit.prevent>
        <div class="login-title">蓝鲤网盘</div>

        <!--输入电子邮箱-->
        <el-form-item prop="email">
          <el-input 
          size="large" 
          clearable 
          placeholder="请输入电子邮箱地址" 
          v-model.trim="formData.email" 
          maxLength="150">
            <template #prefix>
              <el-icon>
                <User />
              </el-icon>
            </template>
          </el-input>
        </el-form-item>

        <!--输入登录密码-->
        <el-form-item label="" prop="password" v-if="opType == 1">
          <el-input 
          size="large" 
          type="password" 
          clearable placeholder="请输入密码" 
          v-model.trim="formData.password" 
          maxLength="150"
            show-password>
            <template #prefix>
              <el-icon>
                <Lock />
              </el-icon>
            </template>
          </el-input>
        </el-form-item>


          <!-- 注册或者重置密码时 -->
        <div v-if="opType == 0 || opType == 2">
          <!--邮箱验证码-->
          <el-form-item label="" prop="emailCode">
            <div class="send-email-panel">
              <el-input 
              size="large" 
              placeholder="请输入电子邮箱验证码" 
              v-model.trim="formData.emailCode"
              >
                <template #prefix>
                  <el-icon><CircleCheck /></el-icon>
                </template>
              </el-input>
              <el-button class="send-mail-btn" type="primary" size="large" round @click="sendEmailCode">
                获取验证码
              </el-button>
            </div>

            <!-- <el-popover placement="left" :width="500" trigger="click">
              <div>
                <p>1、在垃圾箱中查找邮箱验证码</p>
                <p>2、在邮箱中头像->设置->反垃圾->白名单->设置邮件地址白名单</p>
                <p>
                  3、将邮箱XXX添加到白名单不知道怎么设置？
                </p>
              </div>
              <template #reference>
                <span class="a-link"  :style="{ 'font-size': '14px'}">
                 未收到邮箱验证码？
                </span>
              </template>
            </el-popover> -->
          </el-form-item>

          <!-- 昵称 -->
            <el-form-item label="" prop="nickName" v-if="opType == 0" >
              <el-input  
              size="large"
              placeholder="请输入昵称" 
              v-model.trim="formData.nickName"
              maxLength="20"
              >
                <template #prefix>
                  <el-icon><User /></el-icon>
                </template>
              </el-input>
            </el-form-item>


        <!--注册密码、找回密码-->
          <el-form-item  prop="registerPassword" >
            <el-input 
            type="password"
            size="large"
            placeholder="请输入密码"
            v-model.trim="formData.registerPassword"
            show-password
            >
              <template #prefix>
                <el-icon><Lock /></el-icon>
              </template>
            </el-input>
          </el-form-item>
          <!-- 重复密码 -->
          <el-form-item  prop="reRegisterPassword" >
            <el-input 
            type="password" 
            size="large"
            placeholder="请确认密码"
            v-model.trim="formData.reRegisterPassword"
            show-password
            >
              <template #prefix>
                <el-icon><Lock /></el-icon>
              </template>
            </el-input>
          </el-form-item>

        </div>



        <!--输入验证码-->
        <el-form-item prop="checkCode">
          <div class="check-code-panel">
            <el-input size="large" 
            clearable placeholder="请输入验证码" 
            v-model.trim="formData.checkCode"
            @keyup.enter="doSubmit"
            >
              <template #prefix>
                <el-icon>
                  <CircleCheck />
                </el-icon>
              </template>
            </el-input>
            <!-- 验证码 -->
            <div height="44px">
              <checkCode ref="checkCodeRef"></checkCode>
            </div>
          </div>
        </el-form-item>

        <!-- 登录 -->
        <el-form-item  v-if="opType == 1">
          <div class="rememberme-panel">
            <el-checkbox v-model="formData.remenberMe">记住我</el-checkbox>
          </div>
          <div class="no-account">
            <a href="javascript:void(0)" class="a-link" @click="showPanel(2)">
              忘记密码？</a>
            <a href="javascript:void(0)" class="a-link" @click="showPanel(0)">
              没有账号</a>
          </div>
        </el-form-item>

        <!-- 找回密码 -->
        <el-form-item  v-if="opType == 2">
            <a href="javascript:void(0)" class="a-link" @click="showPanel(1)">
              去登录</a>
        </el-form-item>


        <el-form-item  v-if="opType == 0">
            <a href="javascript:void(0)" class="a-link" @click="showPanel(1)">
              已有账号</a>
        </el-form-item>



        <!--登录按钮-->
        <el-form-item label="" prop="">
          <el-button type="primary" class="op-btn" size="large" round  @click="doSubmit">
            <span  v-if="opType == 0">注册</span>
            <span  v-if="opType == 1">登录</span>
            <span  v-if="opType == 2">重置密码</span>
          </el-button>
        </el-form-item>

      </el-form>
    </div>
    
    <!-- <Dialog
      :show="dialogConfig4SendMailCode.show"
      :title="dialogConfig4SendMailCode.title"
      :buttons="dialogConfig4SendMailCode.buttons"
      width="600px"
      :showCancel="false"
      @close="dialogConfig4SendMailCode.show = false">

      <el-form
        :model="formData4SendMailCode"
        :rules="rules"
        ref="formData4SendMailCodeRef"
        label-width="80px"
        @submit.prevent>
        <el-form-item label="邮箱" prop="" >
          {{formData.email}}
        </el-form-item>

        <el-form-item label="验证码" prop="checkCode" >
            <div class="check-code-panel">
            <el-input 
            size="large" 
            clearable placeholder="请输入验证码" 
            v-model.trim="formData4SendMailCode.checkCode">
              <template #prefix>
                <el-icon>
                  <CircleCheck />
                </el-icon>
              </template>
            </el-input>
            
            <div height="44px">
              <checkCode></checkCode>
            </div>
          </div>
          </el-form-item>

      </el-form>      

    </Dialog> -->

  </div>
</template>



<script setup>

import { useRouter ,useRoute} from "vue-router";
import md5 from "js-md5";
import axios from 'axios';


const router = useRouter();
const route = useRoute();

import { ref, reactive, getCurrentInstance, nextTick, onMounted } from "vue"
const { proxy } = getCurrentInstance();

import checkCode from "../components/checkCode.vue";
//import { pa } from "element-plus/es/locale";


// const api = {
//   // checkCode: ""//api地址
//   sendEmailCode: "/api/users/register",
  
//   register: "/register",
//   login: "/login",
//   resetPwd: "/resetPwd",

// }

//检查确认密码是否与上一次输入密码一致
const checkRePassword = (rule , value , callback)=>{
  if(value!==formData.value.registerPassword){
    callback(new Error(rule.message));
  }else{
    callback();
  }
}


//验证码验证
const checkCheckCode = (rule , value , callback)=>{
  if(value!==checkCodeRef.value.codeList.map(item => item.code).join('')){
    callback(new Error(rule.message));
  }else{
    callback();
  }
}


const formData = ref({});
const formDataRef = ref();
const rules = {
  // title: [{ required: true, message: "请输入内容" }],
  email: [{ required: true, message : "请输入邮箱"},
  { validator: proxy.Verify.email , message:"请输入正确的邮箱"},
],
  password: [{ required: true, message : "请输入密码"}],
  emailCode: [{ required: true, message : "请输入邮箱验证码"}],
  nickName: [{ required: true, message : "请输入用户名"}],
  registerPassword: [{ required: true, message : "请输入密码"},
  { validator: proxy.Verify.password , message:"密码只能是数字、字母、特殊字符,8-18位"},
],
  reRegisterPassword: [{ required: true, message : "请再次输入密码"},
  { validator: checkRePassword , message:"与前一次密码不一致"},
],

  checkCode: [{ required: true, message : "请输入验证码"},
  { validator: checkCheckCode , message:"验证码错误"}],
};



//操作类型；0：注册   1：登录    2：重置密码
const opType = ref(1); //一开始默认登录界面
const showPanel = (type) => {
  opType.value = type;
  restForm();
}

onMounted(() => {
  showPanel(1);
})





components: {
  checkCode
}




//发送邮箱验证码
const sendEmailCode = ()=>{
  formDataRef.value.validateField("email", async (valid)=>{
    if(!valid){
      return;
    }
    axios.get('/api/users/register',{
      params:{
        email:formData.email
        // email:formDataRef.value.email
      }
    })
    .then(function(response){
      console.log(response);
      proxy.Message.success("验证码发送成功");
    }
    )
    .catch(function(error){
      console.log(error);
    })
  });
  
}


//验证码引用
const checkCodeRef=ref();

//重置表单
const restForm = ()=>{
  //刷新验证码
  if(checkCodeRef.value){
    checkCodeRef.value.refreshCode();
  }
  //重置表单
  formDataRef.value.resetFields();
  formData.value = {};
  //登录情况下
  if(opType.value == 1){
    const cookieLoginInfo = proxy.VueCookies.get("loginInfo");
    if(cookieLoginInfo){
      formData.value = cookieLoginInfo;
    }
  }

}


//登录、注册、重置密码提交表单
const doSubmit = ()=>{
  formDataRef.value.validate(async (valid)=>{
    if(!valid){
      return;
    }
    if(opType.value == 0){
      //注册
    axios.post('/api/users/register',{
        username:formData.nickName,
        password: formData.password,
        email:formData.email,
        code: formData.emailCode,
    })
    .then(function(response){
      console.log(response);
      proxy.Message.success("注册成功");
      showPanel(1);
    })
    .catch(function(error){
      console.log(error);
    })


    }else if(opType.value == 1){
      //登录
    axios.post('/api/users/login',{
        email:formData.email,
        password: formData.password
    })
    .then(function(response){
      console.log(response);
      proxy.Message.success("登录成功");
      router.push("/");
    })
    .catch(function(error){
      console.log(error);
    })

    }else if(opType.value == 2){
      //修改密码
      
    }

  });
  
};


</script>






<style lang="scss" scoped>
.login-body {
  height: calc(100vh);
  background-size: cover;
  background: url("../assets/login_background.png");
  display: flex;

  .bg {
    flex: 1;
    background-size: cover;
    background-position: center;
    background-size: 800px;
    background-repeat: no-repeat;
    background-image: url("../assets/login_pic.png");
  }

  .login-panel {
    width: 430px;
    margin-right: 15%;
    margin-top: calc((100vh - 500px)/2);

    .login-register {
      padding: 25px;
      background: #fff;
      border-radius: 5px;

      .login-title {
        text-align: center;
        font-size: 18px;
        font-weight: bold;
        margin-bottom: 20px;
      }

      .send-email-panel {
        display: flex;
        width: 100%;
        justify-content: space-between;

        .send-mail-btn {
          margin-left: 5px;
        }
      }

      .rememberme-panel {
        width: 100%;
      }

      .no-account {
        width: 100%;
        display: flex;
        justify-content: space-between;
      }

      .op-btn {
        width: 100%;
      }
    }
  }

  .check-code-panel {
    width: 100%;
    display: flex;

    .check-code {
      margin-left: 5px;
      cursor: pointer;
    }
  }

  // qq不要
}
</style>