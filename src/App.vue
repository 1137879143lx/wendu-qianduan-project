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
          <h1 style="color: white">温湿度检测云平台1</h1>
        </div>
        <el-menu
          :default-active="activeIndex"
          @select="handleSelect"
          class="el-menu-vertical-demo"
          style="background-color: #0c2135"
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
          <!-- <h1>温湿度检测云平台</h1> -->
        </el-header>
        <el-main>Main</el-main>
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
  },
};
</script>

<style>
.el-menu-vertical-demo {
  height: calc(100vh);
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
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 25px;
}
</style>
