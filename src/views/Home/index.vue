<template>
  <div class="Home">
    <div
      style="
        display: flex;
        justify-content: space-between;
        height: 60vh;

        margin-bottom: 15px;
      "
    >
      <div style="width: 50%; display: flex; flex-direction: column">
        <div style="height: 100%">
          <el-card
            header="监控列表 17项"
            shadow="never"
            style="
              height: 100%;
              color: #909399;
              border-radius: 10px;
              /* background: linear-gradient(to right, #8a5ebe, #5f388c); */
            "
          >
            <!-- 监控列表 -->
            <!-- <el-scrollbar height="400px">
              <div
                v-for="item in 8"
                :key="item"
                class="scrollbar-demo-item"
                style="
                  display: flex;
                  height: 40px;
                  align-items: center;
                  border-bottom: 1px solid rgb(179, 161, 161);
                "
              >
                <div>{{ item }}</div>
                <el-icon :size="22" style="margin-left: 8px"
                  ><Filter
                /></el-icon>
                <div
                  style="
                    margin-left: 5px;
                    display: flex;
                    flex-direction: column;
                    width: 100px;
                  "
                >
                  <div>检测室1号</div>
                  <div style="font-size: 12px; color: #d1caca">
                    192.168.33.3
                  </div>
                </div>

                <div style="margin-left: 30px">温度 24.32 ℃</div>
                <div style="margin-left: 30px">湿度 68.98 %</div>
              </div>
            </el-scrollbar> -->
            <el-table size="mini" :data="tableData" style="width: 100%">
              <el-table-column type="index" label="#" width="50" />
              <el-table-column prop="name" label="名称" width="120" />
              <el-table-column prop="isalarm" label="状态" width="120" />
              <el-table-column prop="temperature" label="温度" width="100" />
              <el-table-column prop="humidity" label="湿度" width="100" />
            </el-table>
          </el-card>
        </div>
        <!-- <div style="height: 55%">
          <el-card
            header="报警记录"
            shadow="never"
            style="
              border-radius: 10px;
              background-color: #e9e4ed;
              margin-top: 15px;
              height: 100%;
            "
            >报警记录</el-card
          >
        </div> -->
      </div>
      <div style="width: 50%; margin-left: 15px">
        <div style="display: flex; flex-direction: column; height: 100%">
          <div style="height: 80%">
            <el-card
              shadow="never"
              style="
                border-radius: 10px;
                background-color: #e9e4ed;
                height: 100%;
              "
              >表格曲线</el-card
            >
          </div>
          <div style="height: 20%">
            <el-card
              shadow="never"
              style="
                border-radius: 10px;
                background-color: #e9e4ed;
                margin-top: 15px;
                height: 100%;
              "
              >最高最低</el-card
            >
          </div>
        </div>
      </div>
    </div>
    <div style="padding-top: 15px; height: 20vh">
      <el-card
        shadow="never"
        style="background-color: #e9e4ed; border-radius: 10px; height: 100%"
        >Investments</el-card
      >
    </div>
  </div>
</template>

<style>
* {
  padding: 0%;
  margin: 0%;
}
</style>

<script>
export default {
  name: "Home-home",

  data() {
    return {
      tableData: [
        {
          name: "测量室1",
          mac: "192.168.0.32",
          temperature: 16.87,
          humidity: 45.66,
          maxTemperature: 24.32,
          minTemperature: 12.32,
          maxHumidity: 68.98,
          minHumidity: 45.98,
          isalarm: false,
        },
      ],
    };
  },

  methods: {},
  mounted() {},
};
</script>
