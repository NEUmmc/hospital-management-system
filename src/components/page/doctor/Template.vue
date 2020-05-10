<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>医生工作站</el-breadcrumb-item>
      <el-breadcrumb-item>模版管理</el-breadcrumb-item>
    </el-breadcrumb>
    <br />
    <br />
    <!-- 两边的整体布局 -->
    <div class="Div">
      <!-- 左侧div -->
      <div style="width:400px">
        <div class="head-input">
          <el-link :underline="false">模版名:</el-link>
          <el-input style="width:200px" placeholder="搜索" size="small">
            <el-button slot="append" icon="el-icon-search"></el-button>
          </el-input>

          <el-button size="small" icon="el-icon-refresh-right" type="danger" @click="refresh"></el-button>
        </div>
        <br />
        <br />

        <div class="Div">
          <el-link :underline="false">模版列表:</el-link>
          <el-button size="small" type="danger" @click="dialogVisible = true">新增模版</el-button>
        </div>
        <!-- <el-tag type="warning">待诊患者</el-tag> -->
        <br />

        <el-table :data="tableData" size="mini" style="width: 100%">
          <el-table-column prop="name" label="模版名称" width="70"></el-table-column>
          <el-table-column prop="time" label="创建时间" width="70"></el-table-column>
          <el-table-column prop="fan" label="范围" width="50"></el-table-column>
          <el-table-column prop="type" label="类型" width="50"></el-table-column>
          <el-table-column label="操作" width="150">
            <template slot-scope="scope">
              <el-button size="small" @click="look(scope.row)">查看</el-button>
              <el-button size="small" type="danger" @click="del(scope.$index)">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
        <br />
        <br />
      </div>

      <!-- 右侧div -->
      <div style="width: 1100px">
        <!-- 顶部文字div -->
        <el-link :underline="false" type="danger">模版信息:</el-link>
        <el-divider></el-divider>

        <div class="head-text">
          <span>模版名称：{{Sname}}</span>
          <span>创建时间：{{Stime}}</span>
          <span>范围：{{Sfan}}</span>
          <span>类型：{{Stype}}</span>
        </div>

        <br />
        <br />
        <el-card class="box-card" style="width: 100%">
          <div slot="header">
            <span>模版明细：</span>
            <el-button
              style="float: right; padding: 3px 0"
              type="text"
              @click="dialogVisible3 = true"
            >增加模版项目</el-button>
          </div>
          <div>
            <el-table :data="preTable">
              <el-table-column prop="itemCode" label="项目编码"></el-table-column>
              <el-table-column prop="itemName" label="项目名称"></el-table-column>
              <el-table-column prop="itemFormat" label="项目规格"></el-table-column>
              <el-table-column prop="part" label="部位"></el-table-column>
              <el-table-column prop="type" label="类型"></el-table-column>
              <el-table-column label="操作">
                <template slot-scope="scope">
                  <el-button
                    size="small"
                    type="text"
                    @click.native.prevent="deleteRow(scope.$index, preTable)"
                  >移除</el-button>
                </template>
              </el-table-column>
            </el-table>
          </div>
        </el-card>

        <br />
        <br />
        <div class="bottom-b">
          <el-button type="danger" @click="save">保存</el-button>
          <el-button @click="can">取消</el-button>
        </div>
      </div>
    </div>
    <el-dialog title="删除处方" :visible.sync="dialogVisible2" width="20%" :before-close="handleClose">
      确认删除处方？
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible2 = false">取 消</el-button>
        <el-button type="danger" @click="sureDel">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="添加项目" :visible.sync="dialogVisible3" width="60%" :before-close="handleClose">
      <el-table :data="itemTable">
        <el-table-column prop="itemCode" label="项目编码"></el-table-column>
        <el-table-column prop="itemName" label="项目名称"></el-table-column>
        <el-table-column prop="itemFormat" label="项目规格"></el-table-column>
        <el-table-column prop="part" label="部位"></el-table-column>
        <el-table-column prop="type" label="类型"></el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button type="text" @click="useItem(scope.row)">添加</el-button>
          </template>
        </el-table-column>
      </el-table>

      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible3 = false">取 消</el-button>
        <el-button type="danger" @click="dialogVisible3 = false">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="新增模版" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
      <span>
        模版名称：
        <el-input v-model="name" size="small" style="width:270px"></el-input>
      </span>
      <br />
      <br />
      <span>
        创建时间：
        <el-input v-model="time" size="small" style="width:270px"></el-input>
      </span>
      <br />
      <br />
      <span>
        范围：
        <el-input v-model="fan" size="small" style="width:300px"></el-input>
      </span>
      <br />
      <br />
      <span>
        类型：
        <el-input v-model="type" size="small" style="width:300px"></el-input>
      </span>
      <br />
      <br />
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="danger" @click="newTem">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  inject: ["reload"],
  data() {
    return {
      name: "",
      time: "",
      fan: "",
      type: "",
      Sname: "",
      Stime: "",
      Sfan: "",
      Stype: "",

      preTable: [],
      itemTable: [
        {
          itemCode: "1234",
          itemName: "CT",
          itemFormat: "次",
          part: "胸部",
          type: "检查"
        },
        {
          itemCode: "1222",
          itemName: "洗胃",
          itemFormat: "次",
          part: "胃",
          type: "检查"
        },
        {
          itemCode: "1234",
          itemName: "CT",
          itemFormat: "次",
          part: "胸部",
          type: "检查"
        },
        {
          itemCode: "1234",
          itemName: "CT",
          itemFormat: "次",
          part: "胸部",
          type: "检查"
        }
      ],
      dialogVisible: false,

      dialogVisible2: false,
      dialogVisible3: false,

      row: "",

      tableData: []
    };
  },
  mounted() {
    this.tableData = [
      {
        name: "小儿感冒",
        time: "2018-9-9",
        fan: "科室",
        type: "处方"
      }
    ];
  },
  methods: {
    can() {
      this.Sname = "";
      this.Stime = "";
      this.Sfan = "";
      this.Stype = "";
      this.preTable = [];
      this.$message.success("已取消");
    },
    save() {
      this.$message.success("保存成功");
    },
    del(row) {
      this.dialogVisible2 = true;
      this.row = row;
    },
    useItem(row) {
      this.preTable.push(row);
    },
    newTem() {
      this.dialogVisible = false;
      this.tableData.push({
        name: this.name,
        time: this.time,
        fan: this.fan,
        type: this.type
      });
      this.$message.success("新增成功");
    },

    sureDel() {
      this.$message.success("删除成功");
      this.deleteRow(this.row, this.tableData);
      this.dialogVisible2 = false;
    },
    clear() {
      this.$message.success("已作废");
    },
    deleteRow(index, rows) {
      rows.splice(index, 1);
    },

    look(row) {
      this.Sname = row.name;
      this.Stime = row.time;
      this.Sfan = row.fan;
      this.Stype = row.type;
    },

    refresh() {
      this.reload();
      this.$message.success("已刷新");
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
.Div {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.head-input {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 100%;
}
.head-text {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 60%;
  height: 32px;
  align-items: center;
}
.head-button {
  margin: 25px 250px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 40%;
}
.bottom-b {
  position: absolute;
  right: 20px;
}
</style>