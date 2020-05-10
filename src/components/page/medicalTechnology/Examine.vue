<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>医技工作站</el-breadcrumb-item>
      <el-breadcrumb-item>患者检验</el-breadcrumb-item>
    </el-breadcrumb>
    <br />
    <br />
    <!-- 两边的整体布局 -->
    <div class="Div">
      <!-- 左侧div -->
      <div style="width:360px">
        <div class="head-input">
          <el-link :underline="false">患者姓名:</el-link>
          <el-input style="width:200px" placeholder="搜索" size="small">
            <el-button slot="append" icon="el-icon-search"></el-button>
          </el-input>

          <el-button size="small" icon="el-icon-refresh-right" type="danger" @click="refresh"></el-button>
        </div>
        <br />

        <br />
        <el-link :underline="false" type="warning">未检验患者:</el-link>

        <!-- <el-tag type="warning">待诊患者</el-tag> -->
        <br />
        <br />
        <el-table :data="tableData" size="mini" style="width: 100%" max-height="250">
          <el-table-column prop="records_num" label="病历号" width="100"></el-table-column>
          <el-table-column prop="name" label="姓名" width="100"></el-table-column>
          <el-table-column prop="age" label="年龄" width="75"></el-table-column>
          <el-table-column label="操作">
            <template slot-scope="scope">
              <el-button size="small" type="danger" @click="dia(scope.row)">检验</el-button>
            </template>
          </el-table-column>
        </el-table>
        <br />
        <br />
        <el-link :underline="false" type="success">已检验患者:</el-link>

        <!-- <el-tag type="success">已诊患者</el-tag> -->
        <br />
        <br />
        <el-table :data="tableData1" size="mini" style="width: 100%" max-height="250">
          <el-table-column prop="records_num" label="病历号" width="100"></el-table-column>
          <el-table-column prop="name" label="姓名" width="100"></el-table-column>
          <el-table-column prop="age" label="年龄" width="75"></el-table-column>
          <el-table-column label="操作">
            <template slot-scope="scope">
              <el-button size="small" type="danger" @click="look(scope.row)">查看</el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>

      <!-- 右侧div -->
      <div>
        <!-- 顶部文字div -->
        <div class="head-text">
          <el-link :underline="false" type="danger">当前患者:</el-link>
          <span>{{Srecords_num}}</span>
          <span>{{Sname}}</span>
          <span>{{Sage}}</span>
        </div>
        <br />
        <!-- 右侧tab -->
        <el-tabs v-model="activeName" style="width: 1150px">
          <el-tab-pane :disabled="paneDisable" label="患者信息" name="first">
            <br />
            <el-link :underline="false">基本信息</el-link>
            <el-divider></el-divider>
            <span>{{Sname}}</span>
            <br />
            <br />
            <span>{{Sage}}</span>
            <br />
            <br />
            <span>性别：男</span>
            <br />
            <br />
            <span>身份证号：140227199903020090</span>
            <br />
            <br />
            <br />
            <el-link :underline="false">历史病例</el-link>
            <el-divider></el-divider>
            <span>既往史：</span>
            <br />
            <br />
            <span>个人史：</span>
            <br />
            <br />
            <span>过敏史：</span>
          </el-tab-pane>

          <el-tab-pane :disabled="paneDisable" label="检验项目">
            <el-card class="box-card" style="width: 98%">
              <div slot="header">
                <span>未检验项目</span>
              </div>
              <div>
                <el-table :data="checkTable" style="width: 100%">
                  <el-table-column prop="itemName" label="项目名称"></el-table-column>
                  <el-table-column prop="department" label="检验科室"></el-table-column>
                  <el-table-column prop="price" label="缴费金额"></el-table-column>
                  <el-table-column label="操作">
                    <template slot-scope="scope">
                      <el-button type="danger" size="small" @click="dialogVisible=true">确定</el-button>

                      <el-button size="small" @click="cancelDialog(scope.$index)">取消</el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </div>
            </el-card>
            <br />
            <br />
            <el-card class="box-card" style="width: 98%">
              <div slot="header">
                <span>已检验项目</span>
              </div>
              <div>
                <el-table :data="checkTable1" style="width: 100%">
                  <el-table-column prop="itemName" label="项目名称"></el-table-column>
                  <el-table-column prop="department" label="检验科室"></el-table-column>
                  <el-table-column prop="price" label="缴费金额"></el-table-column>
                  <el-table-column label="操作">
                    <template>
                      <el-button type="danger" size="small" @click="dialogVisible1=true">录入结果</el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </div>
            </el-card>
          </el-tab-pane>
        </el-tabs>
      </div>
    </div>
    <el-dialog title="提示" :visible.sync="dialogVisible" width="25%" :before-close="handleClose">
      <span>确认执行该项目？</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="danger" @click="sure">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="提示" :visible.sync="dialogVisible2" width="25%" :before-close="handleClose">
      <span>确认取消该项目？</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible2 = false">取 消</el-button>
        <el-button type="danger" @click="cancel">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="录入结果" :visible.sync="dialogVisible1" width="50%" :before-close="handleClose">
      <span>项目名称：鼻镜</span>
      <br />
      <br />
      <span>检验医师：吕新慧</span>
      <br />
      <br />
      <span>
        检验结果：
        <br />
        <br />
        <el-input type="textarea" :rows="2" placeholder="请输入结果" v-model="textarea"></el-input>
      </span>
      <br />
      <br />
      <span>
        检验图片：
        <br />
        <br />
        <el-upload
          action="https://jsonplaceholder.typicode.com/posts/"
          list-type="picture-card"
          :on-preview="handlePictureCardPreview"
          :on-remove="handleRemove"
        >
          <i class="el-icon-plus"></i>
        </el-upload>
        <el-dialog :visible.sync="dialogVisibleImg">
          <img width="100%" :src="dialogImageUrl" alt />
        </el-dialog>
      </span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible1 = false">取 消</el-button>
        <el-button type="danger" @click="save">保存</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  inject: ["reload"],
  data() {
    return {
      textarea: "",
      count: 0,
      paneDisable: true,
      dialogVisible: false,
      dialogVisible1: false,
      dialogVisible2: false,
      dialogImageUrl: "",
      dialogVisibleImg: false,
      activeName: "",

      Srecords_num: "",
      Sname: "",
      Sage: "",

      row: "",

      tableData: [],
      tableData1: [],

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
        }
      ],
      checkTable1: []
    };
  },
  mounted() {
    this.tableData = [
      {
        records_num: 600600,
        name: "李白",
        age: 32
      },
      {
        records_num: 600601,
        name: "张三",
        age: 33
      },
      {
        records_num: 600602,
        name: "李四",
        age: 23
      }
    ];
    this.tableData1 = [
      {
        records_num: 600600,
        name: "赵六",
        age: 32
      },
      {
        records_num: 600600,
        name: "王八",
        age: 32
      }
    ];
  },
  methods: {
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisibleImg = true;
    },
    cancelDialog(row) {
      this.dialogVisible2 = true;
      this.row = row;
    },
    save() {
      this.$message.success("已保存");
      this.count += 1;
      this.dialogVisible1 = false;
      if (this.count == 2) {
        this.tableData1.push(this.tableData.shift());
      }
    },
    sure() {
      this.checkTable1.push(this.checkTable.shift());
      this.dialogVisible = false;
    },
    cancel(row) {
      this.deleteRow(row, this.checkTable);
      this.dialogVisible2 = false;
    },
    useItem(row) {
      this.preTable.push(row);
    },
    submit() {
      this.$message.success("提交成功");
      this.dialogVisible1 = false;
    },

    deleteRow(index, rows) {
      rows.splice(index, 1);
    },
    refresh() {
      this.reload();
      this.$message.success("已刷新");
    },
    dia(row) {
      this.Srecords_num = "病历号：" + row.records_num;
      this.records_num = row.records_num;
      this.Sname = "姓名：" + row.name;
      this.Sage = "年龄：" + row.age;
      this.activeName = "first";
      this.paneDisable = false;
    },
    look(row) {
      this.Srecords_num = "病历号：" + row.records_num;
      this.records_num = row.records_num;
      this.Sname = "姓名：" + row.name;
      this.Sage = "年龄：" + row.age;
      this.activeName = "first";
      this.paneDisable = false;
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
  width: 40%;
  height: 32px;
  align-items: center;
}
</style>