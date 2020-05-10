<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>挂号收费</el-breadcrumb-item>
      <el-breadcrumb-item>退号</el-breadcrumb-item>
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

      <!-- <span>病历号：<el-input v-model="records_num" style="width:195px" placeholder="请输入病历号"></el-input></span> -->
      <div>
        <el-button icon="el-icon-search" type="danger" @click="search">搜索</el-button>
        <el-button icon="el-icon-delete" type="danger" @click="clear">清空</el-button>
      </div>
    </div>
    <br />
    <br />
    <span>患者挂号信息（未诊断未退号）：</span>
    <br />
    <br />
    <el-table
      :data="tableData.slice((currentPage-1)*pagesize,currentPage*pagesize)"
      style="width:100%"
    >
      <el-table-column prop="records_num" label="病历号" width="200"></el-table-column>
      <el-table-column prop="name" label="姓名" width="200"></el-table-column>
      <el-table-column prop="idCardNum" label="身份证号" width="250"></el-table-column>
      <el-table-column prop="regTime" label="挂号时间" sortable width="200"></el-table-column>
      <el-table-column
        prop="department"
        label="挂号科室"
        width="200"
        :filters="[{ text: '内科', value: '内科' }, { text: '外科', value: '外科' }]"
        :filter-method="filterHandler"
      ></el-table-column>
      <el-table-column
        prop="doctor"
        label="医生"
        width="200"
        :filters="[{ text: '扁鹊', value: '扁鹊' }, { text: '华佗', value: '华佗' }]"
        :filter-method="filterHandler"
      ></el-table-column>
      <el-table-column prop="state" label="挂号状态" width="200"></el-table-column>

      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="small" type="danger" @click="cancel(scope.row,scope.$index)">退号</el-button>
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

    <el-dialog title="退号操作" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
      <span>信息核对：</span>
      <br />
      <br />
      <span>姓名：{{name}}</span>
      <br />
      <br />
      <span>身份证号：{{idCard}}</span>
      <br />
      <br />
      <el-link :underline="false" type="danger">需退挂号费：20¥</el-link>
      <br />
      <br />
      <span>是否确定退号？</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="danger" @click="sureCancel">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  inject: ["reload"],
  data() {
    return {
      index: "",
      records_num: "",
      name: "",
      idCard: "",
      tableData: [
        {
          records_num: 600600,
          name: "李白",
          name: "李白",
          idCardNum: "140287198802010010",
          regTime: "2020-02-01",
          department: "内科",
          doctor: "华佗",
          state: "未诊断"
        },
        {
          records_num: 600600,
          name: "李白",
          name: "李白",
          idCardNum: "140287198802010010",
          regTime: "2020-01-01",
          department: "内科",
          doctor: "华佗",
          state: "未诊断"
        }
      ],
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
        // this.tableData = resp.data.data;
      }
    },
    cancel(row, index) {
      this.index = index;
      this.name = row.name;
      this.idCard = row.idCardNum;
      this.dialogVisible = true;
    },
    sureCancel() {
      this.dialogVisible = false;
      this.tableData.splice(this.index, 1);
      this.$message.success("退号成功");
    },
    handleSizeChange(size) {
      //一页显示多少条
      this.pagesize = size;
    },
    handleCurrentChange(currentPage) {
      //页码更改方法
      this.currentPage = currentPage;
    },
    handleClose(done) {
      this.$confirm("确认关闭？")
        .then(_ => {
          done();
        })
        .catch(_ => {});
    },
    filterHandler(value, row, column) {
      const property = column["property"];
      return row[property] === value;
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