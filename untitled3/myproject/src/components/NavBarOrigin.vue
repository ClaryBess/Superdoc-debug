// 这是登录前导航栏组件
<template>
  <div>
    <div class="tabBar">
      <router-link to="/" ><img class="logo" src="../assets/nav-logo.png"  alt=""/></router-link>
      <div class="right">

        <ul class="rightNav">
          <li>
            <a class="login" @click="dialogFormVisible = true" style="text-align: center">登录</a>
          </li>
          <li>
            <router-link to="/Register1" class="write">
              <i class="iconfont iconbussiness-man-fill"></i>
              注册
            </router-link>
          </li>
        </ul>

      </div>

    </div>
    <el-dialog title="登录" :visible.sync="dialogFormVisible" width="40%" center>
      <div class="LoginForm">
        <el-form :model="form" :label-position="labelPos">
          <el-form-item label="用户名" style="margin-bottom: 15px" >
            <el-col span=24>
              <el-input placeholder="USERNAME" v-model="form.userName" autocomplete="off" clearable></el-input>
            </el-col>
          </el-form-item>
          <el-form-item label="密码" style="margin-bottom: 40px">
            <el-col span="24">
              <el-input placeholder="PASSWORD" v-model="form.password" autocomplete="off" show-password></el-input>
            </el-col>
          </el-form-item>
        </el-form>
        <SliderVerificationCode v-model="slider" background="#CCCCCC"/>
      </div>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="loginForm('ruleForm')">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  import axios from "axios";
  export default {
    name: "NavBarOrigin",
    inject: ['reload'],
    data() {
      return {
        search:'search',
        dialogFormVisible: false,
        labelPos: 'right',
        form: {
          userName: '',
          password: ''
        },
        slider: false
      }
    },
    methods: {
      inputFocus: function() {
        this.search = 'searchLong'
      },
      inputBlur: function() {
        this.search = 'search'
      },
      loginForm(formName){
        var _this=this
        //axios 需要 import,下面form 对象要与上面view 填写的一样
        axios.post("/user/login",{
          userName:this.form.userName,
          password:this.form.password,
        })
          .then(function (response) {
            // console.log(response.data.status)
            if(response.data.status === 200){
              //alert("恭喜你，登录成功")
                _this.$message({
                message: '恭喜你，登录成功',
                type: 'success'
              })
              sessionStorage.setItem('userL', JSON.stringify(response.data.data))
              sessionStorage.setItem('teamL', null);
              if(_this.$route.name === 'welcome'){
                _this.$router.push('WorkSpace');
              }
              else{
                _this.reload();
              }
            }else {
              _this.$message({
                message: '用户名或密码输入错误！',
                type: 'error'
              })
            }
          }).catch(function (error) {
          console.log(error)
        })
      },
    }
  }
</script>

<style scoped>
  .LoginForm{
    margin: 0 auto;
    width: 80%;
  }
  .tabBar {
    height: 56px;
    width: 100%;
    border-bottom-color: #f2f2f2;
    border-bottom-style: solid;
    background-color: #ffffff;
    border-bottom-width: 2px;
    display: flex;
    z-index: 9999;
  }

  .logo {
    height: 56px;
  }

  li {
    list-style: none;
  }

  a {
    text-decoration: none;
    display: block;
  }

  .nav {
    width: 225px;
    margin-left: 100px;
  }

  .nav li {
    width: 65px;
    height: 56px;
    float: left;
  }

  .nav>li>a {
    width: 65px;
    line-height: 56px;
    color: #333;
    text-align: center;
    font-size: 17px;
  }

  .nav li:hover {
    background-color: #f5f5f5;
  }

  .subNav {
    height: 350px;
    width: 180px;
    visibility: hidden;
    border: #eeeeee solid 1px;
    background-color: #ffffff;
    box-shadow: 0 2px 2px #eeeeee;
  }

  .subNav li:first-child {
    margin-top: 7px;
  }

  .subNav li {
    height: 48px;
    width: 180px;
  }

  .nav>li:last-child:hover .subNav {
    visibility: visible;
  }

  .subNav li a {
    width: 180px;
    height: 48px;
    background-color: #ffffff;
    line-height: 48px;
    color: #333333;
  }

  .subNav li span,
  .subNav li i {
    margin-left: 15px;
  }

  .subNav li a:hover {
    background-color: #f5f5f5;
  }

  .search,
  .searchLong {
    height: 38px;
    background-color: #eee;
    margin: auto 0;
    display: flex;
    align-items: center;
    flex-direction: row-reverse;
    border-radius: 38px;
    transition: width 0.5s;
    transition-delay: 0.5s;
    -webkit-transition-delay: 0.5s;
    -webkit-transition: width 0.5s;
  }

  .search {
    width: 160px;
  }

  .searchLong {
    width: 220px;
  }

  .search i,.searchLong i {
    display: block;
    width: 30px;
    height: 30px;
    line-height: 30px;
    color: rgb(156, 149, 149);
  }

  .searchLong i {
    text-align: center;
    border-radius: 15px;
    color: #fff;
    margin-right: 5px;
    line-height: 30px;
    background-color: #969696;
  }

  .searchInput {
    width: 100%;
    height: 38px;
    padding-left: 15px;
    background: none;
    border: 0px;
    outline: none;
    font-size: 15px;
  }

  ::-webkit-input-placeholder {
    color: #a1a1a1;
  }

  ::-moz-placeholder {
    color: #a1a1a1;
  }

  /* firefox 19+ */
  :-ms-input-placeholder {
    color: #a1a1a1;
  }

  /* ie */
  input:-moz-placeholder {
    color: #a1a1a1;
  }

  .right {
    flex: 1;
    margin-right: 80px;
    display: flex;
    flex-direction: row-reverse;
  }

  .rightNav li {
    height: 56px;
    float: left;
    line-height: 56px;
  }

  .rightNav li:first-child a {
    width: 65px;
    color: #333;
  }

  .rightNav .login:hover{
    background-color: #f5f5f5;
  }

  .write {
    width: 100px;
    height: 40px;
    display: inline-block;
    background-color: #ffffff;
    color: #50BFFF;
    font-size: 15px;
    line-height: 40px;
    text-align: center;
    border-radius: 20px;
    margin-left: 30px;
    border-color: #50BFFF;
    border-style: solid;
    border-width: thin;
  }

  .write:hover {
    border-color: #409EFF;
    color: #409EFF;
    transition: color 0.7s;
  }

  .dropdown {
    width: 90px;
    margin: 0 20px;
  }

  .dropdown>a {
    display: flex;
    flex-direction: row;
    align-items: center;
    height: 56px;
    justify-content: center;
    color: #333;
  }

  .dropdown>a i {
    color: rgb(156, 149, 149);
  }

  .userhead {
    width: 40px;
    height: 40px;
    border: 1px solid #ddd;
    border-radius: 50%;
  }

  .dropdown:hover {
    background-color: #f5f5f5;
  }

  .dropdownNav {
    width: 180px;
    height: 420px;
    visibility: hidden;
    border: #eeeeee solid 1px;
    background-color: #ffffff;
    box-shadow: 0 2px 2px #eeeeee;
  }

  .dropdownNav li:first-child {
    margin-top: 10px;
  }

  .dropdown:hover .dropdownNav {
    visibility: visible;
  }

  .dropdownNav li {
    width: 180px;
    height: 50px;
    line-height: 50px;
    background-color: #ffffff;
  }

  .dropdownNav li:hover {
    background-color: #f5f5f5;
  }

  .dropdownNav li a span,
  .dropdownNav li a i {
    color: #333;
    margin-left: 15px;
  }



  html, body, div, span, applet, object, iframe,
  h1, h2, h3, h4, h5, h6, p, blockquote, pre,
  a, abbr, acronym, address, big, cite, code,
  del, dfn, em, img, ins, kbd, q, s, samp,
  small, strike, strong, sub, sup, tt, var,
  b, u, i, center,
  dl, dt, dd, ol, ul, li,
  fieldset, form, label, legend,
  table, caption, tbody, tfoot, thead, tr, th, td,
  article, aside, canvas, details, embed,
  figure, figcaption, footer, header, hgroup,
  menu, nav, output, ruby, section, summary,
  time, mark, audio, video {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
  }
  /* HTML5 display-role reset for older browsers */
  article, aside, details, figcaption, figure,
  footer, header, hgroup, menu, nav, section {
    display: block;
  }
  body {
    line-height: 1;
  }
  ol, ul {
    list-style: none;
  }
  blockquote, q {
    quotes: none;
  }
  blockquote:before, blockquote:after,
  q:before, q:after {
    content: '';
    content: none;
  }
  table {
    border-collapse: collapse;
    border-spacing: 0;
  }
</style>
