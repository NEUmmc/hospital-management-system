<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>医生工作站</el-breadcrumb-item>
      <el-breadcrumb-item>门诊医生</el-breadcrumb-item>
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
        <!-- 左侧tab -->
        <el-tabs>
          <el-tab-pane label="本人">
            <br />
            <el-link :underline="false" type="warning">待诊患者:</el-link>

            <!-- <el-tag type="warning">待诊患者</el-tag> -->
            <br />
            <br />
            <el-table :data="tableData" size="mini" style="width: 100%" max-height="250">
              <el-table-column prop="records_num" label="病历号" width="100"></el-table-column>
              <el-table-column prop="name" label="姓名" width="100"></el-table-column>
              <el-table-column prop="age" label="年龄" width="75"></el-table-column>
              <el-table-column label="操作">
                <template slot-scope="scope">
                  <el-button size="small" type="danger" @click="dia(scope.row)">诊断</el-button>
                </template>
              </el-table-column>
            </el-table>
            <br />
            <br />
            <el-link :underline="false" type="success">已诊患者:</el-link>

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
          </el-tab-pane>
          <el-tab-pane label="科室">
            <br />
            <el-link :underline="false" type="warning">待诊患者:</el-link>

            <!-- <el-tag type="warning">待诊患者:</el-tag> -->
            <br />
            <br />
            <el-table :data="tableData" size="mini" style="width: 100%" max-height="250">
              <el-table-column prop="records_num" label="病历号" width="100"></el-table-column>
              <el-table-column prop="name" label="姓名" width="100"></el-table-column>
              <el-table-column prop="age" label="年龄" width="75"></el-table-column>
              <el-table-column label="操作">
                <template slot-scope="scope">
                  <el-button size="small" type="danger" @click="dia(scope.row)">诊断</el-button>
                </template>
              </el-table-column>
            </el-table>
            <br />
            <br />
            <el-link :underline="false" type="success">已诊患者:</el-link>

            <!-- <el-tag type="success">已诊患者:</el-tag> -->
            <br />
            <br />
            <el-table :data="tableData1" size="mini" style="width: 100%" max-height="250">
              <el-table-column prop="records_num" label="病历号" width="100"></el-table-column>
              <el-table-column prop="name" label="姓名" width="100"></el-table-column>
              <el-table-column prop="age" label="年龄" width="75"></el-table-column>
              <el-table-column label="操作">
                <template slot-scope="scope">
                  <el-button size="small" type="danger" @click="drug(scope.row)">查看</el-button>
                </template>
              </el-table-column>
            </el-table>
          </el-tab-pane>
        </el-tabs>
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
          <el-tab-pane :disabled="paneDisable" label="病历首页" name="first"></el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="检查申请"></el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="检验申请"></el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="门诊确诊"></el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="处置申请"></el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="成药处方">
            <div class="head-button">
              <el-button
                type="success"
                icon="el-icon-success"
                @click="dialogVisible3 = true"
                size="small"
                plain
              >开立新处方</el-button>
              <el-button
                type="danger"
                icon="el-icon-delete-solid"
                @click="clear"
                size="small"
                plain
              >作废处方</el-button>
              <el-button
                type="primary"
                icon="el-icon-s-order"
                @click="dialogVisible1 = true"
                size="small"
                plain
              >引用套组</el-button>
              <el-button
                type="warning"
                icon="el-icon-s-promotion"
                size="small"
                @click="dialogVisible2 = true"
                plain
              >发送处方</el-button>
            </div>
            <br />
            <br />
            <div class="Div">
              <div>
                <span>处方列表：</span>
                <br />
                <br />
                <el-table :data="preListTable" style="width: 300px">
                  <el-table-column prop="name" label="处方名称"></el-table-column>
                  <el-table-column label="操作">
                    <template slot-scope="scope">
                      <el-button size="small" type="danger" @click="change(scope.row)">修改</el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </div>

              <el-card class="box-card" style="width: 760px">
                <div slot="header">
                  <span>处方明细：{{name}}</span>
                  <el-button
                    style="float: right; padding: 3px 0"
                    type="text"
                    @click="dialogVisible = true"
                  >增加药品</el-button>
                </div>
                <div>
                  <el-table :data="preTable" style="width: 800px">
                    <el-table-column prop="drugsName" label="药品名称" width="120"></el-table-column>
                    <el-table-column prop="drugsFormat" label="规格" width="120"></el-table-column>
                    <el-table-column prop="usage" label="用法" width="80"></el-table-column>
                    <el-table-column prop="dosage" label="用量" width="80"></el-table-column>
                    <el-table-column prop="frequency" label="频次" width="80"></el-table-column>
                    <el-table-column prop="price" label="单价" width="80"></el-table-column>
                    <el-table-column prop="num" label="数量" width="80"></el-table-column>
                    <el-table-column label="操作" width="80">
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
            </div>
          </el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="草药处方">
            <div class="head-button">
              <el-button
                type="success"
                icon="el-icon-success"
                @click="dialogVisible3 = true"
                size="small"
                plain
              >开立新处方</el-button>
              <el-button
                type="danger"
                icon="el-icon-delete-solid"
                @click="clear"
                size="small"
                plain
              >作废处方</el-button>
              <el-button
                type="primary"
                icon="el-icon-s-order"
                @click="dialogVisible1 = true"
                size="small"
                plain
              >引用套组</el-button>
              <el-button
                type="warning"
                icon="el-icon-s-promotion"
                size="small"
                @click="dialogVisible2 = true"
                plain
              >发送处方</el-button>
            </div>
            <br />
            <br />
            <div class="Div">
              <div>
                <span>处方列表：</span>
                <br />
                <br />
                <el-table :data="preListTable" style="width: 300px">
                  <el-table-column prop="name" label="处方名称"></el-table-column>
                  <el-table-column label="操作">
                    <template slot-scope="scope">
                      <el-button size="small" type="danger" @click="change(scope.row)">修改</el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </div>

              <el-card class="box-card" style="width: 760px">
                <div slot="header">
                  <span>处方明细：{{name}}</span>
                  <el-button
                    style="float: right; padding: 3px 0"
                    type="text"
                    @click="dialogVisible = true"
                  >增加药品</el-button>
                </div>
                <div>
                  <el-table :data="preTable" style="width: 800px">
                    <el-table-column prop="drugsName" label="药品名称" width="120"></el-table-column>
                    <el-table-column prop="drugsFormat" label="规格" width="120"></el-table-column>
                    <el-table-column prop="usage" label="用法" width="80"></el-table-column>
                    <el-table-column prop="dosage" label="用量" width="80"></el-table-column>
                    <el-table-column prop="frequency" label="频次" width="80"></el-table-column>
                    <el-table-column prop="price" label="单价" width="80"></el-table-column>
                    <el-table-column prop="num" label="数量" width="80"></el-table-column>
                    <el-table-column label="操作" width="80">
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
            </div>
          </el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="费用查询">
            <br />
            <br />
            <el-table :data="tableData5" style="width: 100%">
              <el-table-column prop="records_num" label="病历号"></el-table-column>
              <el-table-column prop="name" label="姓名"></el-table-column>
              <el-table-column prop="idCardNum" label="身份证号"></el-table-column>
              <el-table-column prop="itemsName" label="项目名称"></el-table-column>
              <el-table-column prop="price" label="单价"></el-table-column>
              <el-table-column prop="num" label="数量"></el-table-column>
              <el-table-column prop="state" label="状态"></el-table-column>
            </el-table>
          </el-tab-pane>
        </el-tabs>
      </div>
    </div>
    <el-dialog title="发送处方" :visible.sync="dialogVisible2" width="20%" :before-close="handleClose">
      确认发送处方
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible2 = false">取 消</el-button>
        <el-button type="danger" @click="send">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="引用套组" :visible.sync="dialogVisible1" width="50%" :before-close="handleClose">
      <div class="fl">
        <div>
          <span>处方模板：</span>
          <br />
          <br />
          <el-table :data="templateTable" size="mini" style="width: 240px">
            <el-table-column prop="name" label="模板名称" width="150"></el-table-column>
            <el-table-column label="操作">
              <template slot-scope="scope">
                <el-button size="small" type="danger" @click="template(scope.row)">查看</el-button>
              </template>
            </el-table-column>
          </el-table>
        </div>

        <div>
          <el-card class="box-card" style="width: 525px">
            <div slot="header">
              <span>模板明细</span>
              <el-button style="float: right; padding: 3px 0" type="text" @click="useTem">使用模板</el-button>
            </div>
            <div>
              <el-table :data="DtemplateTable" size="mini" style="width: 502px">
                <el-table-column prop="drugsName" label="药品名称" width="120"></el-table-column>
                <el-table-column prop="drugsFormat" label="规格" width="120"></el-table-column>
                <el-table-column prop="usage" label="用法" width="80"></el-table-column>
                <el-table-column prop="dosage" label="用量" width="80"></el-table-column>
                <el-table-column prop="frequency" label="频次" width="80"></el-table-column>
              </el-table>
            </div>
          </el-card>
        </div>
      </div>

      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible1 = false">取 消</el-button>
      </span>
    </el-dialog>

    <el-dialog title="添加药品" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
      <span>药品名称</span>
      <el-select v-model="drugs_id" filterable placeholder="请选择药品">
        <el-option v-for="item in drugs" :key="item.value" :label="item.label" :value="item.value"></el-option>
      </el-select>
      <br />
      <br />
      <span>
        用法：
        <el-input v-model="usage" size="small" style="width:200px"></el-input>
      </span>
      <br />
      <br />
      <span>
        用量：
        <el-input v-model="dosage" size="small" style="width:200px"></el-input>
      </span>
      <br />
      <br />
      <span>
        频次：
        <el-input v-model="frequency" size="small" style="width:200px"></el-input>
      </span>
      <br />
      <br />
      <span>
        数量：
        <el-input v-model="num" size="small" style="width:200px"></el-input>
      </span>
      <br />
      <br />
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="danger" @click="add">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="开立新处方" :visible.sync="dialogVisible3" width="30%" :before-close="handleClose">
      <span>
        处方名称：
        <el-input v-model="preName" size="small" style="width:300px"></el-input>
      </span>
      <br />
      <br />
      <span>
        创建医生：
        <el-input v-model="doctor" size="small" style="width:300px"></el-input>
      </span>
      <br />
      <br />
      <span>
        创建时间：
        <el-input v-model="time1" size="small" style="width:300px"></el-input>
      </span>
      <br />
      <br />
      <span>
        修改时间：
        <el-input v-model="time2" size="small" style="width:300px"></el-input>
      </span>
      <br />
      <br />
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible3 = false">取 消</el-button>
        <el-button type="danger" @click="newPre">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  inject: ["reload"],
  data() {
    return {
      tableData5: [
        {
          records_num: 600600,
          name: "李白",
          idCardNum: 144000403934994,
          itemsName: "抽血",
          price: 90,
          num: 1,
          state: "已开立"
        }
      ],
      records_num: "",
      templateTable: [
        {
          name: "咳嗽"
        },
        {
          name: "感冒"
        },
        {
          name: "发烧"
        }
      ],
      DtemplateTable: [],
      preName: "",
      preTable: [],
      preListTable: [
        {
          name: "风寒"
        }
      ],

      drugs_id: "",

      drugs: [
        {
          label: "阿莫西林",
          value: "阿莫西林"
        },
        {
          label: "福尔马林",
          value: "福尔马林"
        },
        {
          label: "板蓝根",
          value: "板蓝根"
        },
        {
          label: "金银花",
          value: "金银花"
        },
        {
          label: "银花",
          value: "银花"
        }
      ],
      usage: "",
      dosage: "",
      frequency: "",
      num: "",
      doctor: "",
      time1: "",
      time2: "",
      name: "",

      paneDisable: true,
      dialogVisible: false,
      dialogVisible1: false,
      dialogVisible2: false,
      dialogVisible3: false,
      activeName: "",

      Srecords_num: "",
      Sname: "",
      Sage: "",

      tableData: [],
      tableData1: []
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
        records_num: 600600,
        name: "李白",
        age: 32
      },
      {
        records_num: 600600,
        name: "李白",
        age: 32
      },
      {
        records_num: 600600,
        name: "李白",
        age: 32
      }
    ];
    this.tableData1 = [
      {
        records_num: 600600,
        name: "李白",
        age: 32
      },
      {
        records_num: 600600,
        name: "李白",
        age: 32
      },
      {
        records_num: 600600,
        name: "李白",
        age: 32
      },
      {
        records_num: 600600,
        name: "李白",
        age: 32
      }
    ];
  },
  methods: {
    add() {
      this.dialogVisible = false;
      this.preTable.push({
        drugsName: this.drugs_id,
        drugsFormat: "支",
        usage: this.usage,
        dosage: this.dosage,
        frequency: this.frequency,
        price: 30,
        num: this.num
      });
    },
    change(row) {
      this.name = row.name;
    },
    newPre() {
      this.dialogVisible3 = false;
      this.preListTable.push({
        name: this.preName
      });
      this.$message.success("开立成功");
    },

    send() {
      this.$message.success("发送成功");
      this.dialogVisible2 = false;
    },
    clear() {
      this.$message.success("已作废");
    },

    useTem() {
      this.DtemplateTable.forEach(element => {
        this.preTable.push(element);
      });
      this.dialogVisible1 = false;
      this.$message.success("模版使用成功");
    },
    deleteRow(index, rows) {
      rows.splice(index, 1);
    },
    template(row) {
      this.DtemplateTable = [
        {
          drugsName: "止咳糖浆",
          drugsFormat: "支",
          usage: "口服",
          dosage: "一支",
          frequency: "每日一次",
          price: 30,
          num: 3
        },
        {
          drugsName: "止咳胶囊",
          drugsFormat: "颗",
          usage: "口服",
          dosage: "一颗",
          frequency: "每日一次",
          price: 20,
          num: 4
        }
      ];
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
.fl {
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
.head-button {
  margin: 25px 250px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 40%;
}
</style>