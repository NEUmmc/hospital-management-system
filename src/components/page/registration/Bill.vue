<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>挂号收费</el-breadcrumb-item>
      <el-breadcrumb-item>费用查询&发票</el-breadcrumb-item>
    </el-breadcrumb>
    <br />
    <br />
    <div class="head-input">
      <el-dropdown placement trigger="click">
        <span class="el-dropdown-link">
          病历号
          <i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>病历号</el-dropdown-item>
          <el-dropdown-item>身份证号</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
      <el-input v-model="records_num" style="width:195px" placeholder="请输入"></el-input>

      <div>
        <el-button icon="el-icon-search" type="danger" @click="search">搜索</el-button>
        <el-button icon="el-icon-delete" type="danger" @click="clear">清空</el-button>
      </div>
    </div>
    <br />
    <br />
    <el-table
      :data="tableData.slice((currentPage-1)*pagesize,currentPage*pagesize)"
      style="width: 100%"
    >
      <el-table-column prop="bill_num" label="发票号" width="150"></el-table-column>
      <el-table-column prop="records_num" label="病历号" width="150"></el-table-column>
      <el-table-column prop="name" label="姓名" width="150"></el-table-column>
      <el-table-column prop="idCardNum" label="身份证号" width="300"></el-table-column>
      <el-table-column prop="time" label="开立时间" width="200"></el-table-column>
      <el-table-column prop="price" label="金额" width="200"></el-table-column>
      <el-table-column prop="state" label="状态" width="200"></el-table-column>

      <el-table-column label="操作">
        <template>
          <el-button size="small" type="danger" @click="settle()">发票补打</el-button>
          <el-button size="small" type="danger" @click="settle()">发票重打</el-button>
        </template>
      </el-table-column>
    </el-table>
    <br />
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="[2,4,6,7]"
      :page-size="pagesize"
      layout="total,sizes,prev,pager,next,jumper"
      :total="tableData.length"
    ></el-pagination>
  </div>
</template>

<script>
export default {
  inject: ["reload"],
  data() {
    return {
      id: "",
      records_num: "",
      tableData: [],
      returnItem: "",
      sum: 0,
      currentPage: 1, //默认页码为1
      pagesize: 6, //默认一页显示10条
      dialogVisible: false
    };
  },
  mounted() {},
  methods: {
    clear() {
      this.reload();
      this.$message.success("已清空");
    },
    search() {
      if (this.records_num == "") {
        this.$message.warning("请先输入病历号再搜索");
      } else {
        this.tableData = [
          {
            records_num: 6000600,
            bill_num: 13445,
            name: "李白",
            idCardNum: "140227199908090090",
            time: "2020-5-1",
            price: 60,
            state: "已缴费"
          }
        ];
      }
    },

    settle() {
      this.$message.success("正在打印请等待");
    },
    handleSizeChange(size) {
      //一页显示多少条
      this.pagesize = size;
    },
    handleCurrentChange(currentPage) {
      //页码更改方法
      this.currentPage = currentPage;
    }
  }
};
</script>

<style scoped>
.head-input {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  width: 32%;
}
</style>