<template>
  <div>
    <div class="head">
      <el-dropdown @command="fn">
        <span class="el-dropdown-link">
          欢迎您,{{info}}
          <i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item command="a">个人中心</el-dropdown-item>
          <el-dropdown-item command="b">修改密码</el-dropdown-item>
          <el-dropdown-item command="c">退出登录</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
    <p class="date">
      今天是你在阿里的第
      <span class="tips">{{days}}</span>天
    </p>
    <div style="clear:both"></div>
  </div>
</template>

<script>
import {get} from "@/utils/http"
import {removeToken} from "@/utils/auth"
export default {
  data() {
    return {
      time: "",
      info: sessionStorage.getItem("nickname"),
    };
  },
  mounted() {
    get("/in").then((res) => {
      this.time = res.time;
    });
  },
  methods: {
    fn(m) {
      if(m=="c"){
          removeToken();
          this.$router.push("/login")
      }
    },
  },
  computed: {
    days() {
      let target = new Date(this.time);
      let now = new Date();
      return Math.floor((now - target) / 1000 / 60 / 60 / 24);
    },
  },
};
</script>

<style scoped>
.head {
  float: right;
}
.date {
  float: right;
  margin-right: 50px;
}
.tips {
  color: #40a9ff;
  font-size: 24px;
}
</style>