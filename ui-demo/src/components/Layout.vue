<template>
  <el-container class="page">
    <el-header>
      <span class="fl">小U商城后台管理系统</span>
      <span class="fr">
        欢迎：{{ $store.state.userinfo ? $store.state.userinfo.username:'' }}
        <a href="javascript:void(0);" @click="logout">退出</a>
      </span>
    </el-header>
    <el-container>
      <el-aside width="160px">
        <el-menu
          :default-active="defaultActive"
          class="el-menu-vertical-demo"
          background-color="#545c64"
          text-color="#fff"
          router
          active-text-color="#ffd04b"
          unique-opened
        >
        <el-menu-item index="/">
            <i class="el-icon-s-home">首页</i>
        </el-menu-item>
          <el-submenu v-for="item of $store.state.userinfo.menus" :key="item.id" :index="item.title">
            <template slot="title">
              <i :class="item.icon"></i>
              <span>{{ item.title }}</span>
            </template>
            <el-menu-item v-for="subitem of item.children" :key="subitem.id" :index="subitem.url">{{ subitem.title }}</el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
    <el-footer>Footer</el-footer>
  </el-container>
</template>

<script>
export default {
    data() {
        return {
            menus:[],
            defaultActive:''
        }
    },
    mounted(){
        this.defaultActive = this.$route.meta.selected;
    },
    watch:{
        $route(newVal){
            this.defaultActive = newVal.meta.selected
        }
    },
    methods: {
      logout(){
        this.$store.commit('setUserInfo',null)
        this.$router.replace('/login')
      }
    },
};
</script>

<style scoped>
.page {
  width: 100vw;
  height: 100vh;
}
.el-header {
  background-color: skyblue;
  line-height: 60px;
}
.el-aside {
  background-color: #545c64;
}
.el-submenu {
  width: 160px;
}
.el-submenu .el-menu-item {
  min-width: 160px;
}
.el-footer {
  background-color: slateblue;
}
.fr {
  position: absolute;
  right: 20px;
}
</style>