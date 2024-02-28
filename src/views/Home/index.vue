<!-- eslint-disable vue/no-unused-vars -->
<template>
  <div class="Home">
    <div>
      <div>
        <div style="height: 100%">
          <el-card
            shadow="never"
            style="height: 800px; color: #909399; border-radius: 10px"
          >
            <template #header>
              <div class="card-header">
                <span>监控列表</span>
                <el-button
                  style="margin-left: 15px"
                  icon="RefreshRight"
                  circle
                ></el-button>
                <el-button
                  style="margin-left: 15px"
                  icon="Plus"
                  circle
                  @click="dialogTableVisible = true"
                ></el-button>
              </div>
            </template>
            <!-- 监控列表 -->
            <el-table
              size="mini"
              height="700"
              :data="tableData"
              style="width: 100%"
            >
              <el-table-column type="index" label="#" width="50" />
              <el-table-column prop="name" label="名称">
                <template v-slot:default="scope">
                  <div>
                    <el-button type="text">{{ scope.row.name }}</el-button>
                  </div>
                </template>
              </el-table-column>
              <el-table-column prop="State" label="状态">
                <template v-slot:default="scope">
                  <el-tag v-if="scope.row.State" type="success">正常</el-tag>
                  <el-tag v-else type="danger">异常</el-tag>
                </template>
              </el-table-column>
              <el-table-column prop="temperature" label="温度" width="120">
                <template v-slot:default="scope">
                  <div style="display: flex; align-items: center">
                    <el-progress
                      class="custom-progress"
                      :percentage="Number(scope.row.temperature)"
                      color="rgb(255, 204, 0)"
                      :format="(percent) => `${scope.row.temperature}℃`"
                      style="flex: 1"
                    />
                    <el-icon
                      color="green"
                      v-if="scope.row.temperature > scope.row.lastTemperature"
                      ><Top
                    /></el-icon>
                    <el-icon
                      color="#858585"
                      v-if="scope.row.temperature < scope.row.lastTemperature"
                      ><Bottom
                    /></el-icon>
                  </div>
                </template>
              </el-table-column>
              <el-table-column prop="humidity" label="湿度" width="120">
                <template v-slot:default="scope">
                  <div style="display: flex; align-items: center">
                    <el-progress
                      :percentage="Number(scope.row.humidity)"
                      color="rgb(0, 204, 255)"
                      size="small"
                      :format="(percent) => `${scope.row.humidity}%`"
                      style="flex: 1"
                    />
                    <el-icon
                      color="green"
                      v-if="scope.row.humidity > scope.row.lastHumidity"
                      ><Top
                    /></el-icon>
                    <el-icon
                      color="red"
                      v-if="scope.row.humidity < scope.row.lastHumidity"
                      ><Bottom
                    /></el-icon>
                  </div>
                </template>
              </el-table-column>
              <el-table-column label="图表" min-width="400">
                <template v-slot:default>
                  <xianChar
                    :temperatureData="generateData2(40)"
                    :humidityData="generateData2(40)"
                  />
                </template>
              </el-table-column>
              <el-table-column prop="maxTemperature" label="最高℃">
              </el-table-column>
              <el-table-column prop="minTemperature" label="最低℃" />

              <el-table-column prop="maxHumidity" label="最高RH%" />
              <el-table-column prop="minHumidity" label="最低RH%" />
              <!-- 操作 -->
              <el-table-column label="操作" min-width="180">
                <template v-slot:default>
                  <el-button type="text" size="small">查看</el-button>
                  <el-button type="text" size="small">编辑</el-button>
                  <el-button type="text" size="small">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
          </el-card>
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

    <!-- 弹窗 -->
    <el-dialog v-model="dialogTableVisible" title="添加设备" width="45%">
    </el-dialog>
  </div>
</template>

<style>
* {
  padding: 0%;
  margin: 0%;
}
</style>

<script>
import xianChar from "@/components/xian_char/index.vue";
export default {
  name: "Home-home",
  components: {
    xianChar,
  },
  data() {
    return {
      tableData: [{}],
      dialogTableVisible: false,
    };
  },

  methods: {
    // 生成随机数据
    generateData(count) {
      const data = [];
      for (let i = 0; i < count; i++) {
        const temperature = Math.random() * 30;
        const humidity = Math.random() * 100;
        data.push({
          name: `测量室${i + 1}`,
          mac:
            Math.random().toString(36).substring(2, 14) +
            Math.random().toString(36).substring(2, 14),
          temperature: temperature.toFixed(2),
          lastTemperature: (temperature + Math.random() * 5 - 2.5).toFixed(2),
          humidity: humidity.toFixed(2),
          lastHumidity: (humidity + Math.random() * 10 - 5).toFixed(2),
          maxTemperature: (temperature + Math.random() * 5).toFixed(2),
          minTemperature: (temperature - Math.random() * 5).toFixed(2),
          maxHumidity: (humidity + Math.random() * 10).toFixed(2),
          minHumidity: (humidity - Math.random() * 10).toFixed(2),
          State: Math.random() > 0.5,
          time: new Date().toISOString(),
          on_alarm: Math.random() > 0.5,
        });
      }
      return data;
    },

    // 生成随机数据
    generateData2(count) {
      const data = Array.from({ length: count }, (_, i) => ({
        value: [
          new Date().getTime() - (count - i) * 60 * 1000,
          (Math.random() * 30 + 10).toFixed(2),
        ],
      }));
      return data;
    },
    // 添加设备
    addDevice() {},
  },
  mounted() {
    this.tableData = this.generateData(10);
  },
};
</script>

<style scoped>
.custom-progress .el-progress__text {
  font-size: 20px; /* 修改这个值来设置字体大小 */
}
</style>
