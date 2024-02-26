<template>
  <div class="common-layout">
    <el-container>
      <el-aside width="200px">
        <div
          style="
            width: 199px;
            height: 60px;
            background-color: #2b2f3a;
            align-items: center;
            justify-content: center;
            display: flex;
          "
        >
          <h1 style="color: rgb(250, 245, 245)">温湿度检测云平台</h1>
        </div>
        <!--  菜单 -->
        <el-menu
          :collapse="isCollapse"
          :default-active="activeIndex"
          @select="handleSelect"
          class="el-menu-vertical-demo"
          style="background-color: #0c2135"
          active-text-color="#ffd04b"
        >
          <el-menu-item index="1">
            <el-icon><House /></el-icon>
            首页
          </el-menu-item>
          <el-menu-item index="2">
            <el-icon><PieChart /></el-icon>
            数据
          </el-menu-item>
          <el-menu-item index="3">
            <el-icon><Setting /></el-icon>
            设置
          </el-menu-item>
        </el-menu>
      </el-aside>

      <el-container>
        <el-header>
          <!-- <el-button type="text" @click="handleCollapse" style="color: #ffffff"
            ><el-icon
              :class="isCollapse ? 'el-icon-s-unfold' : 'el-icon-s-fold'"
          /></el-button> -->
          <el-page-header :icon="ArrowLeft">
            <template #content>
              <span class="text-large font-600 mr-3"> 首页 </span>
            </template>
          </el-page-header>
        </el-header>
        <el-main> <router-view></router-view> </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      activeIndex: "1", // 当前选中的菜单
      isCollapse: false, // 是否折叠
    };
  },
  // 保存当前选中的菜单
  mounted() {
    this.activeIndex = localStorage.getItem("activeIndex") || "1";
  },
  methods: {
    handleSelect(index) {
      this.activeIndex = index;
      localStorage.setItem("activeIndex", index);
    },
    handleCollapse() {
      this.isCollapse = !this.isCollapse;
    },
  },
};
</script>

<style>
.el-menu-vertical-demo {
  height: calc(100vh - 60px);
  overflow: auto;
}
* {
  padding: 0%;
  margin: 0%;
}
.el-menu-vertical-demo .el-menu-item {
  color: #c9b9b9;
}
.el-menu-vertical-demo .el-menu-item:hover {
  background-color: #263445;
}
.el-menu-vertical-demo .el-menu-item.is-active {
  background: linear-gradient(to right, #006efe, #0037a6);
  color: #ffffff;
}
.el-header {
  /* 上下居中 */
  display: flex;
  align-items: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
</style>
