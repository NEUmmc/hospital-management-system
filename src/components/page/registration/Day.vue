<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>挂号收费</el-breadcrumb-item>
      <el-breadcrumb-item>收费员日结</el-breadcrumb-item>
    </el-breadcrumb>
    <br />
    <br />
    <el-link :underline="false" type="danger">今日收费信息清单</el-link>
    <el-divider></el-divider>
    <el-table :data="tableData" style="width:100%">
      <el-table-column prop="id" label="序号"></el-table-column>
      <el-table-column prop="records_num" label="病历号"></el-table-column>
      <el-table-column prop="name" label="姓名"></el-table-column>
      <el-table-column prop="Num" label="缴费单号"></el-table-column>
      <el-table-column prop="regTime" label="开立时间"></el-table-column>
      <el-table-column prop="price" label="价格"></el-table-column>
    </el-table>
    <br />
    <br />
    <div class="bottom">
      <span>应收：{{sum}}¥</span>

      <span>实收：{{sum}}¥</span>

      <el-button type="danger" @click="settle">结算报帐</el-button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sum: 256,
      tableData: [
        {
          id: 1,
          records_num: 600600,
          name: "李白",
          Num: "1402871988",
          regTime: "2020-01-01",
          price: 123
        },
        {
          id: 2,
          records_num: 600610,
          name: "黄是",
          Num: "1402871983",
          regTime: "2020-01-01",
          price: 133
        }
      ]
    };
  },
  methods: {
    settle() {
      this.sum = 0;
      this.tableData = [];
      this.$message.success("日结完成");
    }
  }
};
</script>

<style>
.bottom {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  width: 30%;
}
</style>