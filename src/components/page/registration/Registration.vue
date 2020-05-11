<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>挂号收费</el-breadcrumb-item>
      <el-breadcrumb-item>现场挂号</el-breadcrumb-item>
    </el-breadcrumb>
    <br />
    <br />
    <div class="head-input">
      <span>
        病历号：
        <el-input
          v-model="records_num"
          style="width:150px"
          placeholder="请输入病历号"
          :disabled="recordsNumDisabled"
        ></el-input>
      </span>
      <div>
        <el-button @click="search">查询记录</el-button>
        <el-button @click="New" :disabled="newDisabled">生成新号</el-button>
      </div>
    </div>
    <br />
    <br />
    <el-link :underline="false" type="danger">基本信息</el-link>
    <span>&nbsp;&nbsp;</span>
    <el-popover
      placement="right"
      title="证件读取中..."
      width="200"
      trigger="click"
      content="请将身份证或医保卡放置在读卡器上"
    >
      <el-button slot="reference" plain size="small" round type="success">证件读取</el-button>
    </el-popover>
    <el-divider></el-divider>
    <div class="input">
      <span>
        患者姓名：
        <el-input v-model="name" style="width:200px" placeholder="请输入姓名"></el-input>
      </span>
      <span>
        患者性别：
        <el-select style="width:215px" v-model="sex" placeholder="请选择">
          <el-option
            v-for="item in sexOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </span>
      <span>
        身份证号：
        <el-input v-model="idCardNum" style="width:200px" placeholder="请输入身份证号"></el-input>
      </span>
    </div>
    <br />
    <div class="input">
      <span>
        家庭住址：
        <el-input v-model="homeAddress" style="width:200px" placeholder="请输入家庭住址"></el-input>
      </span>
      <span>
        出生日期：
        <el-date-picker
          v-model="Birthday"
          value-format="yyyy-MM-dd"
          type="date"
          placeholder="出生日期"
          :clearable="false"
          @change="getage"
          :picker-options="pickerOptions"
        ></el-date-picker>
      </span>
      <span>
        患者年龄：
        <el-input style="width:200px" v-model="age" :disabled="true"></el-input>
      </span>
    </div>
    <br />
    <br />
    <el-link :underline="false" type="danger">挂号信息</el-link>
    <el-divider></el-divider>
    <div class="input">
      <span>
        科室类别：
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
      <span>
        挂号级别：
        <el-select style="width:150px" v-model="reg_level_id" placeholder="请选择">
          <el-option
            v-for="item in levelOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </span>
      <span>
        医生：
        <el-select style="width:150px" v-model="doctor_id" placeholder="请选择" @change="getPrice">
          <el-option
            v-for="item in docOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </span>
    </div>
    <br />
    <br />

    <el-link :underline="false" type="danger">费用结算</el-link>
    <el-divider></el-divider>
    <div class="fee">
      <span>
        是否要病历本：
        <el-radio-group @change="books" v-model="book">
          <el-radio :label="1">是</el-radio>
          <el-radio :label="-1">否</el-radio>
        </el-radio-group>
      </span>
      <span>
        应收金额：
        <el-input style="width:150px" v-model="price" :disabled="true"></el-input>
      </span>
      <div>
        <el-button icon="el-icon-bell" type="danger" :disabled="reg" @click="finish">挂号</el-button>
        <el-button icon="el-icon-delete" type="danger" @click="clear">清空</el-button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  inject: ["reload"],
  data() {
    return {
      people: {
        name: "李白",
        sex: "男",
        idCardNum: "140287198802010010",
        Birthday: "1988-02-01",
        age: 32,
        homeAddress: "辽宁沈阳"
      },
      recordsNumDisabled: false,
      newDisabled: false,
      records_num: "",
      name: "",
      sex: "",
      idCardNum: "",
      Birthday: "",
      age: "",
      homeAddress: "",
      department_type_id: "",
      department_id: "",
      reg_level_id: "",
      doctor_id: "",
      price: 0,
      book: -1,
      reg: false,
      depTypeOptions: [
        {
          value: 1,
          label: "内科"
        },
        {
          value: 2,
          label: "外科"
        },
        {
          value: 3,
          label: "儿科"
        }
      ],
      depOptions: [
        {
          value: 1,
          label: "呼吸内科"
        },
        {
          value: 2,
          label: "消化内科"
        },
        {
          value: 3,
          label: "普通内科"
        }
      ],
      docOptions: [
        {
          value: 1,
          label: "华佗"
        },
        {
          value: 2,
          label: "扁鹊"
        }
      ],
      sexOptions: [
        {
          value: 1,
          label: "男"
        },
        {
          value: 2,
          label: "女"
        }
      ],
      levelOptions: [
        {
          value: 1,
          label: "专家号"
        },
        {
          value: 2,
          label: "普通号"
        }
      ],
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() > Date.now();
        }
      }
    };
  },
  methods: {
    finish() {
      this.$message.success("挂号成功");
      this.reload();
    },
    books() {
      this.price = this.price + this.book;
    },
    clear() {
      this.reload();
      this.$message.success("已清空");
    },
    getPrice() {
      this.price = 20;
    },
    search() {
      if (this.records_num == "") {
        this.$message.warning("查询前请输入病历号！");
      } else {
        if (this.records_num != 600600) {
          this.$message.warning("没有查到病历号，已自动生成新病历号");
          this.New();
          this.recordsNumDisabled = true;
        } else {
          this.newDisabled = true;
          this.recordsNumDisabled = true;
          let info = this.people;
          this.name = info.name;
          this.sex = info.sex;
          this.idCardNum = info.idCardNum;
          this.Birthday = info.Birthday;
          this.age = info.age;
          this.homeAddress = info.homeAddress;
        }
      }
    },
    New() {
      this.records_num = 600789;
      this.recordsNumDisabled = true;
    },
    getage() {
      var birthdays = new Date(this.Birthday.replace(/-/g, "-"));
      var d = new Date();
      var age =
        d.getFullYear() -
        birthdays.getFullYear() -
        (d.getMonth() < birthdays.getMonth() ||
        (d.getMonth() == birthdays.getMonth() &&
          d.getDate() < birthdays.getDate())
          ? 1
          : 0);
      this.age = age;
    }
  }
};
</script>

<style scoped>
.input {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 70%;
}
.fee {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  width: 50%;
}
.head-input {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 30%;
}
</style>
