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
          <el-tab-pane :disabled="paneDisable" label="病历首页" name="first">
            <!-- 顶部按钮 -->
            <div class="head-button">
              <el-button type="primary" icon="el-icon-s-order" @click="save" size="small" plain>暂存</el-button>
              <el-button type="success" icon="el-icon-success" @click="submit" size="small" plain>提交</el-button>
              <el-button
                type="danger"
                icon="el-icon-delete-solid"
                @click="clear"
                size="small"
                plain
              >清空</el-button>
              <el-button type="warning" icon="el-icon-refresh" size="small" plain>刷新</el-button>
            </div>
            <span>
              主&nbsp;&nbsp;&nbsp;&nbsp;诉：&nbsp;&nbsp;&nbsp;
              <el-input v-model="mainSuit" style="width:80%"></el-input>
            </span>
            <br />
            <br />
            <span>
              现病史： &nbsp;&nbsp;&nbsp;
              <el-input v-model="now_disease_history" style="width:80%"></el-input>
            </span>
            <br />
            <br />
            <span>
              治疗史： &nbsp;&nbsp;&nbsp;
              <el-input v-model="treatment_history" style="width:80%"></el-input>
            </span>
            <br />
            <br />
            <span>
              既往史： &nbsp;&nbsp;&nbsp;
              <el-input v-model="previous_disease_history" style="width:80%"></el-input>
            </span>
            <br />
            <br />
            <span>
              过敏史： &nbsp;&nbsp;&nbsp;
              <el-input v-model="allergic_history" style="width:80%"></el-input>
            </span>
            <br />
            <br />
            <span>
              初步诊断：
              <el-dropdown placement trigger="click">
                <span class="el-dropdown-link">
                  西医诊断
                  <i class="el-icon-arrow-down el-icon--right"></i>
                </span>
                <el-dropdown-menu slot="dropdown">
                  <el-dropdown-item>西医诊断</el-dropdown-item>
                  <el-dropdown-item>中医诊断</el-dropdown-item>
                </el-dropdown-menu>
              </el-dropdown>
              <el-input v-model="primary_diagnosis" style="width:73%" placeholder></el-input>
            </span>
            <br />
            <br />
            <span>
              检查建议：
              <el-input v-model="check_advice" style="width:80%"></el-input>
            </span>
            <br />
            <br />
            <span>
              注意事项：
              <el-input v-model="notice" style="width:80%"></el-input>
            </span>
          </el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="检查申请">
            <div class="head-button">
              <el-button type="success" icon="el-icon-success" @click="done" size="small" plain>开立项目</el-button>
              <el-button
                type="danger"
                icon="el-icon-delete-solid"
                @click="clear"
                size="small"
                plain
              >作废项目</el-button>
              <el-button type="primary" icon="el-icon-s-order" @click="save" size="small" plain>存为套组</el-button>
              <el-button type="warning" icon="el-icon-refresh" size="small" plain>刷新</el-button>
            </div>
            <el-link :underline="false">检查申请项目</el-link>
            <el-divider></el-divider>
            <el-card class="box-card" style="width: 98%">
              <div slot="header">
                <span>项目明细</span>
                <el-button
                  style="float: right; padding: 3px 0"
                  type="text"
                  @click="dialogVisible = true"
                >新增项目</el-button>
              </div>
              <div>
                <el-table :data="itemTable" style="width: 100%">
                  <el-table-column prop="name" label="申请名称" width="200"></el-table-column>
                  <el-table-column prop="itemName" label="项目名称" width="200"></el-table-column>
                  <el-table-column prop="depart" label="执行科室" width="100"></el-table-column>
                  <el-table-column prop="state" label="执行状态" width="100"></el-table-column>

                  <el-table-column prop="price" label="单价" width="100"></el-table-column>
                  <el-table-column prop="result" label="检查结果" width="300"></el-table-column>
                  <el-table-column label="操作">
                    <template slot-scope="scope">
                      <el-button
                        size="small"
                        type="text"
                        @click.native.prevent="deleteRow(scope.$index, itemTable)"
                      >移除</el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </div>
            </el-card>
            <br />
            <br />
            <el-link :underline="false">模版</el-link>
            <el-divider></el-divider>
            <div class="Div">
              <div style="width:30%">
                <el-table :data="templateTable" size="mini" height="200" style="width:100%">
                  <el-table-column prop="name" label="模板名称" width="200"></el-table-column>
                  <el-table-column label="操作">
                    <template slot-scope="scope">
                      <el-button size="small" type="danger" @click="template(scope.row)">查看</el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </div>

              <el-card class="box-card" style="width: 60%">
                <div slot="header">
                  <span>模板明细</span>
                  <el-button style="float: right; padding: 3px 0" type="text" @click="useTem">使用模板</el-button>
                </div>
                <div>
                  <el-table :data="DtemplateTable" size="mini" style="width: 100%">
                    <el-table-column prop="itemNum" label="项目编号" width="200"></el-table-column>
                    <el-table-column prop="itemName" label="项目名称" width="200"></el-table-column>
                    <el-table-column prop="price" label="费用"></el-table-column>
                  </el-table>
                </div>
              </el-card>
            </div>
          </el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="检验申请">
            <div class="head-button">
              <el-button type="success" icon="el-icon-success" @click="done" size="small" plain>开立项目</el-button>
              <el-button
                type="danger"
                icon="el-icon-delete-solid"
                @click="clear"
                size="small"
                plain
              >作废项目</el-button>
              <el-button type="primary" icon="el-icon-s-order" @click="save" size="small" plain>存为套组</el-button>
              <el-button type="warning" icon="el-icon-refresh" size="small" plain>刷新</el-button>
            </div>
            <el-link :underline="false">检验申请项目</el-link>
            <el-divider></el-divider>
            <el-card class="box-card" style="width: 98%">
              <div slot="header">
                <span>项目明细</span>
                <el-button
                  style="float: right; padding: 3px 0"
                  type="text"
                  @click="dialogVisible = true"
                >新增项目</el-button>
              </div>
              <div>
                <el-table :data="itemTable" style="width: 100%">
                  <el-table-column prop="name" label="申请名称" width="200"></el-table-column>
                  <el-table-column prop="itemName" label="项目名称" width="200"></el-table-column>
                  <el-table-column prop="depart" label="执行科室" width="100"></el-table-column>
                  <el-table-column prop="state" label="执行状态" width="100"></el-table-column>

                  <el-table-column prop="price" label="单价" width="100"></el-table-column>
                  <el-table-column prop="result" label="检查结果" width="300"></el-table-column>
                  <el-table-column label="操作">
                    <template slot-scope="scope">
                      <el-button
                        size="small"
                        type="text"
                        @click.native.prevent="deleteRow(scope.$index, itemTable)"
                      >移除</el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </div>
            </el-card>
            <br />
            <br />
            <el-link :underline="false">模版</el-link>
            <el-divider></el-divider>
            <div class="Div">
              <div style="width:30%">
                <el-table :data="templateTable" size="mini" height="200" style="width:100%">
                  <el-table-column prop="name" label="模板名称" width="200"></el-table-column>
                  <el-table-column label="操作">
                    <template slot-scope="scope">
                      <el-button size="small" type="danger" @click="template(scope.row)">查看</el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </div>

              <el-card class="box-card" style="width: 60%">
                <div slot="header">
                  <span>模板明细</span>
                  <el-button style="float: right; padding: 3px 0" type="text" @click="useTem">使用模板</el-button>
                </div>
                <div>
                  <el-table :data="DtemplateTable" size="mini" style="width: 100%">
                    <el-table-column prop="itemNum" label="项目编号" width="200"></el-table-column>
                    <el-table-column prop="itemName" label="项目名称" width="200"></el-table-column>
                    <el-table-column prop="price" label="费用"></el-table-column>
                  </el-table>
                </div>
              </el-card>
            </div>
          </el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="门诊确诊">
            <div class="head-button">
              <el-button type="primary" icon="el-icon-s-order" @click="save" size="small" plain>保存</el-button>
              <el-button type="success" icon="el-icon-success" @click="submit" size="small" plain>提交</el-button>
              <el-button
                type="danger"
                icon="el-icon-delete-solid"
                @click="clear"
                size="small"
                plain
              >清空</el-button>
              <el-button type="warning" icon="el-icon-refresh" size="small" plain>刷新</el-button>
            </div>

            <el-link :underline="false" type="primary">初步诊断</el-link>
            <el-divider></el-divider>
            <span>西医诊断: 肺部有病</span>
            <br />
            <br />
            <br />
            <el-link :underline="false" type="danger">检查结果</el-link>
            <el-divider></el-divider>
            <span>肺部小面积阴影</span>
            <br />
            <br />
            <br />
            <el-link :underline="false" type="danger">检验结果</el-link>
            <el-divider></el-divider>
            <span>新冠肺炎核酸检测阳性</span>
            <br />
            <br />
            <br />
            <el-link :underline="false" type="success">确诊结果</el-link>
            <el-divider></el-divider>
            <div class="bottom-input">
              <el-dropdown placement trigger="click">
                <span class="el-dropdown-link">
                  西医诊断
                  <i class="el-icon-arrow-down el-icon--right"></i>
                </span>
                <el-dropdown-menu slot="dropdown">
                  <el-dropdown-item>西医诊断</el-dropdown-item>
                  <el-dropdown-item>中医诊断</el-dropdown-item>
                </el-dropdown-menu>
              </el-dropdown>

              <el-select v-model="value" filterable placeholder="请选择诊断结果" @change="selectResult">
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </div>
            <br />
            <br />
            <el-table :data="resultTable" style="width: 50%">
              <el-table-column prop="disCode" label="ICD编码" width="200"></el-table-column>
              <el-table-column prop="disName" label="名称" width="200"></el-table-column>
              <el-table-column prop="date" label="确诊时间"></el-table-column>
            </el-table>
          </el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="处置申请">
            <div class="head-button">
              <el-button type="success" icon="el-icon-success" @click="done" size="small" plain>开立项目</el-button>
              <el-button
                type="danger"
                icon="el-icon-delete-solid"
                @click="clear"
                size="small"
                plain
              >作废项目</el-button>
              <el-button type="primary" icon="el-icon-s-order" @click="save" size="small" plain>存为套组</el-button>
              <el-button type="warning" icon="el-icon-refresh" size="small" plain>刷新</el-button>
            </div>
            <el-link :underline="false">处置申请项目</el-link>
            <el-divider></el-divider>
            <el-card class="box-card" style="width: 98%">
              <div slot="header">
                <span>项目明细</span>
                <el-button
                  style="float: right; padding: 3px 0"
                  type="text"
                  @click="dialogVisible = true"
                >新增项目</el-button>
              </div>
              <div>
                <el-table :data="itemTable" style="width: 100%">
                  <el-table-column prop="name" label="申请名称" width="200"></el-table-column>
                  <el-table-column prop="itemName" label="项目名称" width="200"></el-table-column>
                  <el-table-column prop="depart" label="执行科室" width="100"></el-table-column>
                  <el-table-column prop="state" label="执行状态" width="100"></el-table-column>

                  <el-table-column prop="price" label="单价" width="100"></el-table-column>
                  <el-table-column prop="result" label="检查结果" width="300"></el-table-column>
                  <el-table-column label="操作">
                    <template slot-scope="scope">
                      <el-button
                        size="small"
                        type="text"
                        @click.native.prevent="deleteRow(scope.$index, itemTable)"
                      >移除</el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </div>
            </el-card>
            <br />
            <br />
            <el-link :underline="false">模版</el-link>
            <el-divider></el-divider>
            <div class="Div">
              <div style="width:30%">
                <el-table :data="templateTable" size="mini" height="200" style="width:100%">
                  <el-table-column prop="name" label="模板名称" width="200"></el-table-column>
                  <el-table-column label="操作">
                    <template slot-scope="scope">
                      <el-button size="small" type="danger" @click="template(scope.row)">查看</el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </div>

              <el-card class="box-card" style="width: 60%">
                <div slot="header">
                  <span>模板明细</span>
                  <el-button style="float: right; padding: 3px 0" type="text" @click="useTem">使用模板</el-button>
                </div>
                <div>
                  <el-table :data="DtemplateTable" size="mini" style="width: 100%">
                    <el-table-column prop="itemNum" label="项目编号" width="200"></el-table-column>
                    <el-table-column prop="itemName" label="项目名称" width="200"></el-table-column>
                    <el-table-column prop="price" label="费用"></el-table-column>
                  </el-table>
                </div>
              </el-card>
            </div>
          </el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="成药处方"></el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="草药处方"></el-tab-pane>
          <el-tab-pane :disabled="paneDisable" label="费用查询"></el-tab-pane>
        </el-tabs>
      </div>
    </div>
    <el-dialog title="项目列表" :visible.sync="dialogVisible" width="60%" :before-close="handleClose">
      <el-table :data="itemTable1" style="width: 100%">
        <el-table-column prop="itemNum" label="项目编号" width="200"></el-table-column>
        <el-table-column prop="itemName" label="项目名称" width="200"></el-table-column>
        <el-table-column prop="price" label="费用"></el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button type="text" @click="useItem(scope.row)">添加</el-button>
          </template>
        </el-table-column>
      </el-table>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="danger" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="申请单" :visible.sync="dialogVisible1" width="50%" :before-close="handleClose">
      <span>{{Sname}}</span>
      <br />
      <br />
      <span>申请医生：华佗</span>
      <br />
      <br />
      <span>项目列表：</span>
      <br />
      <br />
      <el-table :data="checkTable" style="width: 100%">
        <el-table-column prop="itemNum" label="项目编号" width="300"></el-table-column>
        <el-table-column prop="itemName" label="项目名称" width="300"></el-table-column>
        <el-table-column prop="price" label="费用"></el-table-column>
      </el-table>

      <br />
      <br />
      <el-link :underline="false" type="danger">总金额：{{sum}} ¥</el-link>
      <br />
      <br />
      <span>开立时间：{{currentdate}}</span>

      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible1 = false">取 消</el-button>
        <el-button type="danger" @click="submit">确定开立</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  inject: ["reload"],
  data() {
    return {
      paneDisable: true,
      dialogVisible: false,
      dialogVisible1: false,
      activeName: "",

      Srecords_num: "",
      Sname: "",
      Sage: "",

      tableData: [],
      tableData1: [],

      mainSuit: "",
      now_disease_history: "",
      treatment_history: "",
      previous_disease_history: "",
      allergic_history: "",

      primary_diagnosis: "",

      check_advice: "",
      notice: "",

      currentdate: "",

      templateTable: [
        {
          name: "轻度骨折"
        },
        {
          name: "偏头痛"
        },
        {
          name: "偏头痛"
        }
      ],

      DtemplateTable: [],

      itemTable: [],

      itemTable1: [
        {
          name: "气脑造影",
          depart: "内科",
          state: "待执行",
          result: "无",
          itemNum: "00001",
          itemName: "气脑造影",
          price: 22
        },
        {
          name: "脑室造影",
          depart: "内科",
          state: "待执行",
          result: "无",
          itemNum: 10000,
          itemName: "脑室造影",
          price: 44
        },
        {
          name: "胃镜",
          depart: "内科",
          state: "待执行",
          result: "无",
          itemNum: 10067,
          itemName: "胃镜",
          price: 22
        },
        {
          name: "肠镜",
          depart: "内科",
          state: "待执行",
          result: "无",
          itemNum: 10000,
          itemName: "肠镜",
          price: 44
        }
      ],
      sum: 0,
      checkTable: [],
      options: [
        {
          value: "选项1",
          label: "感冒"
        },
        {
          value: "选项2",
          label: "发烧"
        },
        {
          value: "选项3",
          label: "新冠肺炎"
        },
        {
          value: "选项4",
          label: "胃溃疡"
        },
        {
          value: "选项5",
          label: "胃穿孔"
        }
      ],
      value: "",
      resultTable: []
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
    selectResult() {
      this.resultTable = [
        {
          disCode: "2019-nCov",
          disName: "新冠肺炎",
          date: this.currentdate
        }
      ];
    },
    done() {
      var date = new Date();
      var seperator1 = "-";
      var year = date.getFullYear();
      var month = date.getMonth() + 1;
      var strDate = date.getDate();
      if (month >= 1 && month <= 9) {
        month = "0" + month;
      }
      if (strDate >= 0 && strDate <= 9) {
        strDate = "0" + strDate;
      }
      var currentdate = year + seperator1 + month + seperator1 + strDate;
      this.currentdate = currentdate;

      this.dialogVisible1 = true;
      this.checkTable = this.itemTable;
    },
    useItem(row) {
      this.itemTable.push(row);
    },
    submit() {
      this.$message.success("提交成功");
      this.dialogVisible1 = false;
      this.DtemplateTable = [];
      this.itemTable = [];
    },
    save() {
      this.$message.success("已存储");
    },
    clear() {
      (this.mainSuit = ""),
        (this.now_disease_history = ""),
        (this.treatment_history = ""),
        (this.previous_disease_history = ""),
        (this.allergic_history = ""),
        (this.primary_diagnosis = ""),
        (this.check_advice = ""),
        (this.notice = ""),
        this.$message.success("已清空");
    },

    useTem() {
      this.DtemplateTable.forEach(element => {
        this.itemTable.push(element);
      });
    },
    deleteRow(index, rows) {
      rows.splice(index, 1);
    },
    template(row) {
      this.DtemplateTable = [
        {
          name: "模版:轻度骨折",
          itemNum: 10067,
          itemName: "打石膏",
          price: 22,
          depart: "骨科",
          state: "待执行",
          result: "无"
        },
        {
          name: "模版:轻度骨折",
          itemNum: 10000,
          itemName: "正骨",
          price: 44,
          depart: "骨科",
          state: "待执行",
          result: "无"
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
.bottom-input {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  width: 30%;
}
</style>