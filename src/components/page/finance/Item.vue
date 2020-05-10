<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>财务管理</el-breadcrumb-item>
      <el-breadcrumb-item>费用科目管理</el-breadcrumb-item>
    </el-breadcrumb>
    <br />
    <br />
    <div class="head-input">
      <span>挂号ID：</span>
      <el-input v-model="id" style="width:195px" placeholder="请输入"></el-input>

      <!-- <span>病历号：<el-input v-model="records_num" style="width:195px" placeholder="请输入病历号"></el-input></span> -->
      <div>
        <el-button icon="el-icon-search" type="danger" @click="search">搜索</el-button>
        <el-button icon="el-icon-plus" type="danger" @click="dialogVisible = true">新增</el-button>
      </div>
    </div>

    <br />
    <br />
    <el-table
      :data="tableData.slice((currentPage-1)*pagesize,currentPage*pagesize)"
      style="width:100%"
    >
      <el-table-column prop="id" label="挂号ID"></el-table-column>
      <el-table-column prop="price1" label="西药费"></el-table-column>
      <el-table-column prop="price2" label="中成药费"></el-table-column>
      <el-table-column prop="price3" label="中草药费"></el-table-column>
      <el-table-column prop="price4" label="挂号费"></el-table-column>
      <el-table-column prop="price5" label="诊疗费"></el-table-column>

      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="small" @click="dialogVisible2 = true">更改</el-button>
          <el-button size="small" type="danger" @click="cancel(scope.row,scope.$index)">删除</el-button>
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

    <el-dialog title="删除" :visible.sync="dialogVisible1" width="30%" :before-close="handleClose">
      <span>是否确定删除？</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible1 = false">取 消</el-button>
        <el-button type="danger" @click="sureCancel">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="新增操作" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
      <span>挂号ID：3</span>
      <br />
      <br />
      <span>
        西药费：
        <el-input v-model="price1" style="width:150px"></el-input>
      </span>
      <br />
      <br />
      <span>
        中成药费：
        <el-input v-model="price2" style="width:150px"></el-input>
      </span>
      <br />
      <br />
      <span>
        中草药费：
        <el-input v-model="price3" style="width:150px"></el-input>
      </span>
      <br />
      <br />
      <span>
        挂号费：
        <el-input v-model="price4" style="width:150px"></el-input>
      </span>
      <br />
      <br />
      <span>
        诊疗费：
        <el-input v-model="price5" style="width:150px"></el-input>
      </span>
      <br />
      <br />
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="danger" @click="add">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="更改操作" :visible.sync="dialogVisible2" width="30%" :before-close="handleClose">
      <span>
        西药费：
        <el-input v-model="price1" style="width:150px"></el-input>
      </span>
      <br />
      <br />
      <span>
        中成药费：
        <el-input v-model="price2" style="width:150px"></el-input>
      </span>
      <br />
      <br />
      <span>
        中草药费：
        <el-input v-model="price3" style="width:150px"></el-input>
      </span>
      <br />
      <br />
      <span>
        挂号费：
        <el-input v-model="price4" style="width:150px"></el-input>
      </span>
      <br />
      <br />
      <span>
        诊疗费：
        <el-input v-model="price5" style="width:150px"></el-input>
      </span>
      <br />
      <br />
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible2 = false">取 消</el-button>
        <el-button type="danger" @click="change">确 定</el-button>
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
      id: "",

      price1: "",

      price2: "",
      price3: "",
      price4: "",
      price5: "",
      tableData: [
        {
          id: 1,
          price1: 11,

          price2: 22,
          price3: 33,
          price4: 44,
          price5: 55
        },
        {
          id: 2,

          price1: 11,
          price2: 43,
          price3: 22,
          price4: 33,
          price5: 56
        }
      ],
      currentPage: 1, //默认页码为1
      pagesize: 6, //默认一页显示10条
      dialogVisible: false,
      dialogVisible1: false,
      dialogVisible2: false
    };
  },
  mounted() {},
  methods: {
    change() {
      this.dialogVisible2 = false;
      this.tableData[0] = {
        id: 1,
        price1: this.price1,
        price2: this.price2,
        price3: this.price3,
        price4: this.price4,
        price5: this.price5
      };
    },
    add() {
      this.$message.success("新增成功");
      this.tableData.push({
        id: 3,

        price1: this.price1,
        price2: this.price2,
        price3: this.price3,
        price4: this.price4,
        price5: this.price5
      });
      this.dialogVisible = false;
    },
    search() {
      if (this.records_num == "") {
        this.$message.warning("请先输入挂号ID再搜索");
      } else {
        // this.tableData = resp.data.data;
      }
    },
    cancel(row, index) {
      this.index = index;
      this.name = row.name;
      this.idCard = row.idCardNum;
      this.dialogVisible1 = true;
    },
    sureCancel() {
      this.dialogVisible1 = false;
      this.tableData.splice(this.index, 1);
      this.$message.success("已删除");
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