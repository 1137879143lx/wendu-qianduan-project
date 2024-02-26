<template>
  <div class="common-layout">
    <el-container>
      <el-header> header</el-header>
      <el-container>
        <el-aside width="150px">
          <el-menu default-active="1" class="el-menu-vertical-demo">
            <el-menu-item index="1">首页</el-menu-item>
            <el-menu-item index="2">数据</el-menu-item>
            <el-menu-item index="3">设置</el-menu-item>
          </el-menu>
        </el-aside>
        <el-main>Main</el-main>
      </el-container>
    </el-container>
  </div>
</template>

<style>
.el-menu-vertical-demo {
  height: calc(100vh - 150px);
  overflow: auto;
}
* {
  padding: 0%;
  margin: 0%;
}
</style>
