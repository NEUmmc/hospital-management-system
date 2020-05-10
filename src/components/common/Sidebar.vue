<template>
  <div class="sidebar-container" :class="{'collapse-width': sharedState.state.isCollapse}">
    <div class="logo" :class="{'collapse-logo': sharedState.state.isCollapse}">HIS</div>
    <el-menu
      default-active="/"
      background-color="#FAF3EA"
      active-text-color="#F67E7E"
      class="el-menu-vertical-demo"
      router
      unique-opened
      :collapse="sharedState.state.isCollapse"
    >
      <!-- 如果有子菜单 -->
      <template v-for="item in items">
        <template v-if="item.subs">
          <el-submenu :index="item.index" :key="item.title">
            <template slot="title">
              <i :class="item.icon"></i>
              <span slot="title">{{item.title}}</span>
            </template>
            <el-menu-item v-for="sub in item.subs" :index="sub.index" :key="sub.title">{{sub.title}}</el-menu-item>
          </el-submenu>
        </template>
        <!-- 没有子菜单 -->
        <template v-else>
          <el-menu-item :index="item.index" :key="item.title">
            <i :class="item.icon"></i>
            <span>{{ item.title }}</span>
          </el-menu-item>
        </template>
      </template>
    </el-menu>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sharedState: this.$isCollapse,
      items: [
        {
          icon: "el-icon-message-solid",
          index: "1",
          title: "挂号收费",
          subs: [
            {
              index: "registration",
              title: "挂号"
            },
            {
              index: "cancel",
              title: "退号"
            },
            {
              index: "search",
              title: "挂号信息管理"
            },
            {
              index: "money",
              title: "收费"
            },
            {
              index: "return",
              title: "退费"
            },
            {
              index: "bill",
              title: "费用查询&发票"
            },
            {
              index: "day",
              title: "收费员日结"
            }
          ]
        },

        {
          icon: "el-icon-menu",
          index: "2",
          title: "医生工作站",
          subs: [
            {
              index: "diagnose",
              title: "门诊医生"
            },
            {
              index: "diagnose1",
              title: "门诊医生1"
            },
            {
              index: "template",
              title: "模版管理"
            }
          ]
        },
        {
          icon: "el-icon-s-tools",
          index: "3",
          title: "医技工作站",
          subs: [
            {
              index: "check",
              title: "患者检查"
            },
            {
              index: "examine",
              title: "患者检验"
            },
            {
              index: "do",
              title: "患者处置"
            },
            {
              index: "tec-management",
              title: "医技管理"
            }
          ]
        },
        {
          icon: "el-icon-s-finance",
          index: "4",
          title: "财务管理",
          subs: [
            {
              index: "item",
              title: "费用科目管理"
            },
            {
              index: "checkday",
              title: "门诊日结核对"
            },
            {
              index: "count",
              title: "门诊工作量统计"
            },
            {
              index: "doctor",
              title: "门诊医生工作量"
            }
          ]
        }
      ]
    };
  },
  computed: {
    onRoutes() {
      return this.$route.path.replace("/", "");
    }
  },
  methods: {}
};
</script>

<style scoped>
.collapse-width {
  width: 64px !important;
}
.collapse-logo {
  font-size: 15px !important;
}
.sidebar-container {
  width: 200px;
  height: 100%;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  transition: width 0.3s;
  z-index: 1001;
  overflow: hidden;
}
.logo {
  width: 100%;
  height: 56px;
  line-height: 56px;
  font-size: 30px;
  font-weight: bold;
  text-align: center;
  color: white;
  background-color: #ffbe85;
}
.el-menu-vertical-demo:not(.el-menu--collapse) {
  width: 200px;
  height: 100%;
}
.el-menu-vertical-demo {
  height: 100%;
}
.el-menu {
  border: none;
  height: 100%;
  width: 100% !important;
  overflow-y: auto;
}
/* .el-submenu .el-menu-item{
      background-color: rgb(0, 8, 15)!important;
    } */
</style>