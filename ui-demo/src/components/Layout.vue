<template>
  <el-container class="page">
    <el-header>Header</el-header>
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
          <el-submenu v-for="item of menus" :key="item.id" :index="item.title">
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
import axios from 'axios'
export default {
    data() {
        return {
            menus:[],
            defaultActive:''
        }
    },
    mounted(){
        console.log(this.defaultActive);
        this.defaultActive = this.$route.meta.selected;
        axios.get('/api/menulist',{ params:{istree:true} }).then(result=>{
            this.menus = result.data.list
        })
    },
    watch:{
        $route(newVal){
            this.defaultActive = newVal.meta.selected
        }
    }
};
</script>

<style scoped>
.page {
  width: 100vw;
  height: 100vh;
}
.el-header {
  background-color: skyblue;
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
</style>