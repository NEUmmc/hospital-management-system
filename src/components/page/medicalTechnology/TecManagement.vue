<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>医技工作站</el-breadcrumb-item>
      <el-breadcrumb-item>医技管理</el-breadcrumb-item>
    </el-breadcrumb>
    <br />
    <br />
    <div class="head-input">
      <span>
        项目名称：
        <el-select style="width:150px" v-model="department_type_id" placeholder="请选择">
          <el-option
            v-for="item in depTypeOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </span>
      <span>
        科室名称：
        <el-select style="width:150px" v-model="department_id" placeholder="请选择">
          <el-option
            v-for="item in depOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </span>
      <div>
        <el-button @click="search" type="danger">查询</el-button>
        <el-button @click="dialogVisible1 = true" type="danger">新增</el-button>
      </div>

      <div>
        <el-button>导入医技项目</el-button>
        <el-button>导出医技项目</el-button>
      </div>
    </div>
    <br />
    <br />
    <el-table :data="checkTable" style="width: 100%">
      <el-table-column prop="itemName" label="项目名称"></el-table-column>
      <el-table-column prop="department" label="检验科室"></el-table-column>
      <el-table-column prop="price" label="缴费金额"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button type="danger" size="small" @click="dialogVisible=true">修改</el-button>

          <el-button size="small" @click="cancelDialog(scope.$index)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog title="修改" :visible.sync="dialogVisible" width="25%" :before-close="handleClose">
      <span>项目名称：</span>
      <br />
      <br />
      <span>
        科室：
        <el-select style="width:150px" v-model="departmentChange" placeholder="请选择">
          <el-option
            v-for="item in depOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </span>
      <br />
      <br />
      <span>
        费用：
        <el-input v-model="price" style="width:150px" placeholder="请输入费用"></el-input>
      </span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="danger" @click="sure">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="新增" :visible.sync="dialogVisible1" width="25%" :before-close="handleClose">
      <span>
        项目名称：
        <el-input v-model="newName" style="width:150px" placeholder="请输入名称"></el-input>
      </span>
      <br />
      <br />
      <span>
        科室：
        <el-select style="width:150px" v-model="newDepartment" placeholder="请选择">
          <el-option
            v-for="item in depOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </span>
      <br />
      <br />
      <span>
        费用：
        <el-input v-model="newPrice" style="width:150px" placeholder="请输入费用"></el-input>
      </span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="danger" @click="New">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="提示" :visible.sync="dialogVisible2" width="25%" :before-close="handleClose">
      <span>确认删除该项目？</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible2 = false">取 消</el-button>
        <el-button type="danger" @click="cancel">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  inject: ["reload"],
  data() {
    return {
      price: 300,
      dialogVisible: false,
      dialogVisible1: false,
      dialogVisible2: false,

      department_type_id: "",
      department_id: "",
      departmentChange: "耳鼻科",
      newName: "",
      newDepartment: "",
      newPrice: "",
      checkTable: [
        {
          itemName: "鼻镜",
          department: "耳鼻科",
          price: 300
        },
        {
          itemName: "肠镜",
          department: "内科",
          price: 300
        },
        {
          itemName: "胃镜",
          department: "内科",
          price: 300
        }
      ],
      depTypeOptions: [
        {
          value: 1,
          label: "鼻镜"
        },
        {
          value: 2,
          label: "胃镜"
        },
        {
          value: 3,
          label: "肠镜"
        }
      ],
      depOptions: [
        {
          value: "耳鼻科",
          label: "耳鼻科"
        },
        {
          value: "呼吸内科",
          label: "呼吸内科"
        },
        {
          value: "消化内科",
          label: "消化内科"
        },
        {
          value: "普通内科",
          label: "普通内科"
        }
      ]
    };
  },
  methods: {
    search() {
      if (this.department_type_id == "" && this.department_id == "") {
        this.$message.warning("请先选择！");
      } else {
        this.checkTable = [
          {
            itemName: "鼻镜",
            department: "耳鼻科",
            price: 300
          }
        ];
      }
    },
    New() {
      this.dialogVisible1 = false;
      this.checkTable.push({
        itemName: this.newName,
        department: this.newDepartment,
        price: this.newPrice
      });
      this.$message.success("已新增");
    },
    cancelDialog(row) {
      this.dialogVisible2 = true;
      this.row = row;
    },
    cancel(row) {
      this.deleteRow(row, this.checkTable);
      this.dialogVisible2 = false;
    },
    deleteRow(index, rows) {
      rows.splice(index, 1);
    },
    sure() {
      (this.checkTable = [
        {
          itemName: "鼻镜",
          department: this.departmentChange,
          price: this.price
        },
        {
          itemName: "肠镜",
          department: "内科",
          price: 300
        },
        {
          itemName: "胃镜",
          department: "内科",
          price: 300
        }
      ]),
        (this.dialogVisible = false);
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
  justify-content: space-between;
  width: 60%;
}
</style>
