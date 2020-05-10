<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>挂号收费</el-breadcrumb-item>
      <el-breadcrumb-item>门诊日结核对</el-breadcrumb-item>
    </el-breadcrumb>
    <br />
    <br />
    <div class="head-checkday">
      <span>
        收费员ID：
        <el-select v-model="id" style="width:150px" placeholder="请选择">
          <el-option v-for="item in pids" :key="item.value" :label="item.label" :value="item.value"></el-option>
        </el-select>
      </span>
      <span>
        开始时间：
        <el-date-picker
          v-model="start"
          value-format="yyyy-MM-dd"
          type="date"
          placeholder="开始时间"
          :clearable="false"
          :picker-options="pickerOptions"
        ></el-date-picker>
      </span>

      <span>
        结束时间：
        <el-date-picker
          v-model="end"
          value-format="yyyy-MM-dd"
          type="date"
          placeholder="结束时间"
          :clearable="false"
          :picker-options="pickerOptions"
        ></el-date-picker>
      </span>
    </div>
    <br />
    <br />
    <el-table :data="tableData" style="width:100%">
      <el-table-column prop="id" label="序号"></el-table-column>
      <el-table-column prop="pid" label="收费员ID"></el-table-column>
      <el-table-column prop="name" label="收费员姓名"></el-table-column>
      <el-table-column prop="time" label="开立时间"></el-table-column>
      <el-table-column prop="price" label="结算金额"></el-table-column>
    </el-table>
    <br />
    <br />
    <el-button class="button" type="danger" @click="save">保存</el-button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id: "",
      start: "",
      end: "",
      tableData: [
        {
          id: 1,
          pid: "002",
          name: "白余",
          time: "2020-01-01",
          price: 123
        },
        {
          id: 2,
          pid: "002",
          name: "白余",
          time: "2020-01-01",
          price: 123
        },
        {
          id: 3,
          pid: "002",
          name: "白余",
          time: "2020-01-01",
          price: 123
        },
        {
          id: 4,
          pid: "002",
          name: "白余",
          time: "2020-01-01",
          price: 123
        }
      ],
      pids: [
        {
          value: 1,
          label: "001"
        },
        {
          value: 2,
          label: "002"
        },
        {
          value: 3,
          label: "003"
        },
        {
          value: 4,
          label: "004"
        }
      ],
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() > Date.now();
        }
      }
    };
  },
  methods: {
    save() {
      this.$message.success("已保存");
    }
  }
};
</script>

<style>
.button {
  width: 200px;
  position: absolute;
  right: 100px;
}
.head-checkday {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 70%;
}
</style>