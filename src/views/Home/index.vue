<!-- eslint-disable vue/no-unused-vars -->
<template>
  <div class="Home">
    <div>
      <div>
        <div>
          <el-card shadow="never" style="color: #909399; border-radius: 10px">
            <template #header>
              <div class="card-header">
                <span>监控列表</span>
                <el-tooltip class="box-item" effect="dark" content="刷新" placement="top-start">
                  <el-button style="margin-left: 15px" icon="RefreshRight" circle></el-button>
                </el-tooltip>
                <el-tooltip class="box-item" effect="dark" content="添加设备" placement="top-start">
                  <el-button style="margin-left: 15px" icon="Plus" circle @click="dialogTableVisible = true"></el-button>
                </el-tooltip>
                <el-tooltip class="box-item" effect="dark" content="导出" placement="top-start">
                  <el-button style="margin-left: 15px" icon="Upload" @click="shujudaochu_show = true" circle></el-button>
                </el-tooltip>



              </div>
            </template>
            <!-- 监控列表 -->
            <el-table size="mini" height="75vh" :data="tableData" style="width: 100%">
              <el-table-column type="index" label="#" width="50" />
              <el-table-column prop="name" label="名称" width="150">
                <template v-slot:default="scope">
                  <div>
                    <el-button type="text">{{ scope.row.name }}</el-button>
                  </div>
                </template>
              </el-table-column>
              <el-table-column prop="State" label="状态" width="100">
                <template v-slot:default="scope">
                  <el-tag v-if="scope.row.State" type="success">正常</el-tag>
                  <el-tag v-else type="danger">异常</el-tag>
                </template>
              </el-table-column>
              <el-table-column prop="temperature" label="温度" width="120">
                <template v-slot:default="scope">
                  <div style="display: flex; align-items: center">
                    <el-progress width="90" type="dashboard" class="custom-progress"
                      :percentage="Number(scope.row.temperature)" color="rgb(255, 204, 0)"
                      :format="(percent) => `${scope.row.temperature}℃`" style="flex: 1" />
                    <el-icon color="green" v-if="scope.row.temperature > scope.row.lastTemperature">
                      <Top />
                    </el-icon>
                    <el-icon color="#858585" v-if="scope.row.temperature < scope.row.lastTemperature">
                      <Bottom />
                    </el-icon>
                  </div>
                </template>
              </el-table-column>
              <el-table-column prop="humidity" label="湿度" width="120">
                <template v-slot:default="scope">
                  <div style="display: flex; align-items: center">
                    <el-progress width="80" type="dashboard" :percentage="Number(scope.row.humidity)"
                      color="rgb(0, 204, 255)" size="small" :format="(percent) => `${scope.row.humidity}%`"
                      style="flex: 1" />
                    <el-icon color="green" v-if="scope.row.humidity > scope.row.lastHumidity">
                      <Top />
                    </el-icon>
                    <el-icon color="red" v-if="scope.row.humidity < scope.row.lastHumidity">
                      <Bottom />
                    </el-icon>
                  </div>
                </template>
              </el-table-column>
              <el-table-column label="图表" width="800">
                <template v-slot:default>
                  <xianChar :temperatureData="generateData2(80)" :humidityData="generateData2(80)" />
                </template>
              </el-table-column>
              <!-- <el-table-column prop="maxTemperature" label="最高℃" />
              <el-table-column prop="minTemperature" label="最低℃" />
              <el-table-column prop="maxHumidity" label="最高RH%" />
              <el-table-column prop="minHumidity" label="最低RH%" /> -->
              <!-- 操作 -->
              <el-table-column label="操作" width="180">
                <template v-slot:default>
                  <el-button type="text">查看</el-button>
                  <el-button type="text">编辑</el-button>
                  <el-button type="text">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
          </el-card>
        </div>
      </div>
    </div>

    <!-- 弹窗 -->
    <el-dialog v-model="dialogTableVisible" title="添加设备">
      <el-form label-position="right" label-width="100px" :model="form">
        <el-form-item label="设备名称">
          <el-input v-model="form.name" />
        </el-form-item>
        <el-form-item label="设备MAC">
          <el-input v-model="form.mac" />
        </el-form-item>
        <!-- 采集间隔  分钟-->
        <el-form-item label="采集间隔">
          <el-input-number v-model="form.collectInterval" controls-position="right" :min="1" :max="60" :step="1" />
          <span style="margin: 0 10px">分钟</span>
        </el-form-item>

        <!-- 是否超出范围报警 -->
        <el-form-item label="范围报警">
          <el-switch v-model="form.on_alarm" active-color="#13ce66" />
        </el-form-item>
        <!-- 温度范围选择  -40至80 -->
        <el-form-item label="温度范围" v-if="form.on_alarm">
          <el-slider v-model="form.Temperature_range_selection" range :marks="Temperature_marks" show-stops :max="80"
            :min="-40" />
        </el-form-item>
        <!-- 湿度范围选择  0至100 -->
        <el-form-item label="湿度范围" v-if="form.on_alarm">
          <el-slider v-model="form.Humidity_range_selection" range :marks="Humidity_marks" show-stops :max="100" />
        </el-form-item>
        <!-- 报警方式 -->
        <el-form-item label="报警方式" v-if="form.on_alarm">
          <el-radio-group v-model="form.alarmType">
            <el-radio label="1">邮件</el-radio>
            <!-- <el-radio label="2">短信</el-radio> -->
            <!-- <el-radio label="3">微信</el-radio> -->
          </el-radio-group>
        </el-form-item>
        <!-- 联系方式 -->
        <el-form-item label="联系方式" v-if="form.on_alarm">
          <el-input v-model="form.contact" />
        </el-form-item>

        <!-- 设备描述 -->
        <el-form-item label="设备描述">
          <el-input v-model="form.description" />
        </el-form-item>
      </el-form>
      <template v-slot:footer>
        <span class="dialog-footer">
          <el-button @click="dialogTableVisible = false">取 消</el-button>
          <el-button type="primary" @click="addDevice">确 定</el-button>
        </span>
      </template>
    </el-dialog>
    <!-- 数据导出-->
    <el-dialog title="导出" v-model="shujudaochu_show" width="40%">
      <template v-slot:title>
        <span>导出</span>
        <el-date-picker style="margin-left: 33px;" v-model="Time_range" type="daterange" unlink-panels range-separator="至"
          start-placeholder="开始日期" end-placeholder="结束日期" :shortcuts="shortcuts" />
      </template>
      <!-- 表格 -->
      <el-table :data="tableData" style=" width: 100%;" :fit="false">
        <el-table-column type="index" label="#" />
        <el-table-column prop="name" label="名称" />
        <el-table-column prop="mac" label="MAC" width="200" />
        <el-table-column prop="temperature" label="温度" />
        <el-table-column prop="humidity" label="湿度" />
        <el-table-column prop="State" label="状态">
          <template v-slot:default="scope">
            <el-tag v-if="scope.row.State" type="success">正常</el-tag>
            <el-tag v-else type="danger">异常</el-tag>
          </template>
        </el-table-column>


      </el-table>


      <template v-slot:footer>
        <span class="dialog-footer">
          <el-button @click="shujudaochu_show = false">取 消</el-button>
          <el-button type="primary" @click="addDevice">导 出</el-button>
        </span>
      </template>
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



import { ref } from "vue";

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
      shujudaochu_show: false,
      Time_range: "",
      shortcuts: [
        {
          text: '最近一周',
          value: () => {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)
            return [start, end]
          },
        },
        {
          text: '最近一个月',
          value: () => {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30)
            return [start, end]
          },
        },
        {
          text: '最近三个月',
          value: () => {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90)
            return [start, end]
          },
        },
      ],
      Humidity_marks: {
        0: "0%",
        20: "20%",
        40: "40%",
        60: "60%",
        80: "80%",
        100: "100%",
      },
      Temperature_marks: {
        "-40": "-40℃",
        0: "0℃",
        20: "20℃",
        40: "40℃",
        60: "60℃",
        80: "80℃",
      },


      form: {
        name: "", // 设备名称
        mac: "", // 设备MAC
        collectInterval: 5, // 采集间隔
        on_alarm: false, // 是否超出范围报警
        alarmType: "1", // 报警方式
        contact: "", // 联系方式
        description: "", // 设备描述
        Temperature_range_selection: ref([0, 40]), // 温度范围选择
        Humidity_range_selection: ref([20, 70]), // 湿度范围选择
      },
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
    addDevice() {
      console.log(this.form);
    },
  },
  mounted() {
    this.tableData = this.generateData(10);
  },
};
</script>

<style scoped>
.custom-progress .el-progress__text {
  font-size: 20px;
  /* 修改这个值来设置字体大小 */
}
</style>
