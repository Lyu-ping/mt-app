<template>
  <section class="page-register">
    <article class="header">
      <header>
        <a href="/" class="site-logo"></a>
        <span class="login">
          <em class="bold">已有美团账号？</em>
          <a href="/login">
            <el-button type="primary" size="small">登录</el-button>
          </a>
        </span>
      </header>
    </article>
    <section class="form">
      <el-form ref="form" :model="form" label-width="80px" :rules="rules">
        <el-form-item label="账号" prop="name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="邮箱" prop="email">
          <el-input v-model="form.email"></el-input>
          <el-button size="mini" round @click="sendMsg">发送验证码</el-button>
          <span class="status">{{ statusMsg }}</span>
        </el-form-item>
        <el-form-item label="验证码" prop="code">
          <el-input v-model="form.code" maxlength="4"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="pwd">
          <el-input v-model="form.pwd" type="password"></el-input>
        </el-form-item>
        <el-form-item label="确认密码" prop="cpwd">
          <el-input v-model="form.cpwd" type="password"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="register">同意以下协议并注册</el-button>
          <div class="error">{{error}}</div>
          <a class="f1" href="http://www.meituan.com/about/terms" target="_blank">《美团网用户协议》</a>
          <a
            class="f1"
            href="https://rules-center.meituan.com/rules-detail/2"
            target="_blank"
          >《美团网隐私协议》</a>
        </el-form-item>
      </el-form>
    </section>
  </section>
</template>

<script>
import axios from "axios";
import CryptoJs from "crypto-js";
export default {
  layout: "blank",
  data() {
    return {
      form: {
        name: "",
        email: "",
        code: "",
        cpwd: "",
        pwd: ""
      },
      statusMsg: "",
      error: "",
      rules: {
        name: [
          {
            required: true,
            type: "string",
            message: "请输入账号",
            trigger: "blur"
          }
        ],
        email: [
          {
            required: true,
            type: "email",
            message: "请输入邮箱",
            trigger: "blur"
          }
        ],
        pwd: [
          {
            required: true,
            message: "请输入密码",
            trigger: "blur"
          }
        ],
        cpwd: [
          {
            required: true,
            message: "请再次输入密码",
            trigger: "blur"
          },
          {
            validator: (rule, value, callback) => {
              if (value === "") {
                callback(new Error("请再次输入密码"));
              } else if (value !== this.form.pwd) {
                callback(new Error("两次输入密码不一致"));
              } else {
                callback();
              }
            },
            trigger: "blur"
          }
        ]
      }
    };
  },
  methods: {
    sendMsg() {
      let self = this;
      let namePass;
      let emailPass;
      if (self.timerid) {
        return false;
      }
      self.$refs["form"].validateField("name", valid => {
        namePass = valid;
      });
      self.statusMsg = "";
      if (namePass) {
        return false;
      }
      self.$refs["form"].validateField("email", valid => {
        emailPass = valid;
      });
      if (!namePass && !emailPass) {
        axios
          .post("/users/verify", {
            username: encodeURIComponent(self.form.name),
            email: self.form.email
          })
          .then(({ status, data }) => {
            if (status === 200 && data && data.code === 0) {
              let count = 60;
              self.statusMsg = `验证码已经发送,${count--}秒后可以再次发送`;
              self.timerid = setInterval(() => {
                self.statusMsg = `验证码已经发送,${count--}秒后可以再次发送`;
                if (count === 0) {
                  clearInterval(self.timerid);
                  this.statusMsg = ''
                }
              }, 1000);
            } else {
              self.statusMsg = data.msg;
            }
          });
      }
    },
    register() {
      this.$refs["form"].validate(valid => {
        if (valid) {
          axios
            .post("/users/signup", {
              username: window.encodeURIComponent(this.form.name),
              password: this.form.pwd,
              email: this.form.email,
              code: this.form.code
            })
            .then(({ status, data }) => {
              if (status === 200) {
                if (data && data.code === 0) {
                  location.href = "/login";
                } else {
                  this.error = data.msg;
                }
              } else {
                this.error = `服务器出错`;
              }
              setTimeout(() => {
                this.error = "";
              }, 1500);
            });
        }
      });
    }
  }
};
</script>

<style lang="scss">
@import "@/assets/css/register/index.scss";
</style>