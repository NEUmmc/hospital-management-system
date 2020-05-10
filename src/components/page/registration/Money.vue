<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>挂号收费</el-breadcrumb-item>
      <el-breadcrumb-item>收费</el-breadcrumb-item>
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
    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="records_num" label="病历号" width="200"></el-table-column>
      <el-table-column prop="name" label="姓名" width="200"></el-table-column>
      <el-table-column prop="idCardNum" label="身份证号" width="300"></el-table-column>
      <el-table-column prop="itemsName" label="项目名称" width="250"></el-table-column>
      <el-table-column prop="price" label="单价" width="200"></el-table-column>
      <el-table-column prop="num" label="数量" width="200"></el-table-column>
      <el-table-column prop="state" label="状态"></el-table-column>
    </el-table>

    <br />
    <br />
    <el-link :underline="false" type="danger">总金额：{{sum}} ¥</el-link>
    <el-divider></el-divider>
    <div class="input">
      <span>
        实收金额：
        <el-input v-model="Tm" style="width:200px" placeholder="请输入实收金额" @change="mo"></el-input>
      </span>

      <span>
        找零：
        <el-input v-model="Rm" style="width:200px" :disabled="true"></el-input>
      </span>
      <el-button type="danger" @click="settle">收费结算</el-button>
    </div>
    <el-dialog title="结算" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
      <span>信息核对：</span>
      <br />
      <br />
      <span>姓名：{{name}}</span>
      <br />
      <br />
      <span>身份证号：{{idCard}}</span>
      <br />
      <br />
      <el-link :underline="false" type="danger">总金额：{{sum}} ¥</el-link>
      <br />
      <br />
      <el-link :underline="false" type="danger">实收金额：{{Tm}} ¥</el-link>
      <br />
      <br />
      <el-link :underline="false" type="success">找零：{{Rm}} ¥</el-link>

      <br />
      <br />
      <span>请在收费找零完成后点击确定打印发票</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="danger" @click="sureSettle">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  inject: ["reload"],
  data() {
    return {
      id: "",
      records_num: "",
      name: "",
      idCard: "",
      tableData: [],
      dialogVisible: false,
      sum: 0,
      Tm: "",
      Rm: ""
    };
  },
  mounted() {},
  methods: {
    mo() {
      this.Rm = this.Tm - this.sum;
    },
    clear() {
      this.reload();
      this.$message.success("已清空");
    },
    search() {
      if (this.records_num == "") {
        this.$message.warning("请先输入病历号再搜索");
      } else {
        // "records_num":this.records_num
        // this.tableData =
        // this.sum = resp.data.code + "元";
      }
    },

    settle() {
      // this.name = resp.data.data[0].name;
      // this.idCard = resp.data.data[0].idCard;
      this.dialogVisible = true;
    },
    sureSettle() {
      this.dialogVisible = false;
      // this.search();
      this.$message.success("收费成功");
    },
    handleClose(done) {
      this.$confirm("确认关闭？")
        .then(_ => {
          done();
        })
        .catch(_ => {});
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
.input {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 50%;
}
</style>