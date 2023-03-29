<template>
  <div class="metting-create-record">
    <el-dialog
      :modal="true"
      :close-on-click-modal="false"
      :title="dioTit"
      :visible.sync="isDio"
      custom-class="custom-dialog"
      :width="dioWidth"
      @close="closeDio"
      scr
    >
      <div class="body-container">
        <div id="pdfHtml" class="p65">
          <el-row style="padding-top: 40px">
            <el-col :span="24"
              ><div
                class="tc"
                style="color: black; font-weight: bold; font-size: 32px"
              >
                广东省监狱系统指挥中心日常工作考核评分表
              </div></el-col
            >
          </el-row>
          <el-row class="pt10">
            <el-col :span="24"
              ><div class="tc" style="color: black; font-size: 26px">
                日常工作考核内容
              </div></el-col
            >
          </el-row>
          <el-row class="pt10 pb10">
            <el-col :span="12"
              ><div
                style="color: black; font-weight: bold; font-size: 20px"
                class="tl"
              >
                考核单位：广州监狱
              </div></el-col
            >
            <el-col :span="12"
              ><div
                style="color: black; font-weight: bold; font-size: 20px"
                class="tr"
              >
                评分年月：2022-7
              </div></el-col
            >
          </el-row>
          <el-table
            border
            header-cell-class-name="tableHead"
            :data="tableData"
            :header-cell-style="cells"
            :cell-style="oneStyle"
            :span-method="arraySpanMethod"
          >
            <el-table-column
              label="大项"
              prop="bigItem"
              width="60"
            ></el-table-column>
            <!-- 对应小项 -->
            <el-table-column prop="smellItem" label="项目" width="60">
            </el-table-column>
            <el-table-column label="标准分解">
              <el-table-column prop="orderNumber" label="序号" width="80">
              </el-table-column>
              <el-table-column prop="score" label="分值" width="80">
              </el-table-column>
              <el-table-column prop="content" label="内容"> </el-table-column>
            </el-table-column>
            <el-table-column prop="scoreStandard" label="评分标准">
            </el-table-column>
            <el-table-column prop="checkInfo" label="检查情况">
            </el-table-column>
            <el-table-column prop="decodeScore" label="扣分（分）" width="80">
              <template slot-scope="scope">
                <el-link type="primary" @click="jump">{{
                  scope.row.decodeScore
                }}</el-link>
              </template>
            </el-table-column>
            <el-table-column prop="remark" label="备注" width="100">
            </el-table-column>
          </el-table>
        </div>
      </div>

      <div :class="['center_nav']">
        <template>
          <el-button
            circle
            icon="el-icon-download"
            title="导出PDF"
            @click="handleExportPDF"
          ></el-button>
        </template>
        <el-button
          circle
          icon="el-icon-close"
          title="关闭"
          @click="closeDio()"
        ></el-button>
        <el-backtop
          class="backTop iconfont icon-fanhuidingbu"
          target=".el-dialog.is-fullscreen"
          :visibility-height="0"
          :right="0"
          :bottom="0"
          title="返回顶部"
        ></el-backtop>
      </div>
    </el-dialog>
  </div>
</template>

<script>
// import userOrgTc from '@/components/auth-user-org';

import qs from "qs";
import { apiUrl } from "@/assets/js/api";
import { getPdf } from "@/utils/utils";
import { saveAs } from "file-saver";
let tableColSpan = new Map();

export default {
  components: {},
  data() {
    return {
      dioWidth: "1100px",
      isDio: false, // 弹窗1
      dioTit: "", // 弹窗标题
      dioType: "",
    //   合并格式
      tableData: [
        {
          bigItem: "要情信息流转",
          orderNumber: "4",
          score: "4",
          smellItem: "",
          content: "发生二级要情",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "日常业务",
          orderNumber: "5",
          score: "5",
          smellItem: "监狱要情简报报送",
          content: "及时收集各类",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "要情信息流转",
          orderNumber: "3",
          score: "3",
          smellItem: "",
          content: "发生二级要情",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "日常业务",
          orderNumber: "6",
          score: "3",
          smellItem: "监狱事项报告",
          content: "及时收集各类",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "日常业务",
          orderNumber: "7",
          score: "3",
          smellItem: "信息流转",
          content: "及时收集各类",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "特殊业务",
          orderNumber: "7",
          score: "3",
          smellItem: "",
          content: "及时收集各类",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "日常业务",
          orderNumber: "7",
          score: "3",
          smellItem: "信息月报",
          content: "及时收集各类",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "日常业务",
          orderNumber: "7",
          score: "3",
          smellItem: "信息月报",
          content: "及时收集各类",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "日常业务",
          orderNumber: "7",
          score: "3",
          smellItem: "信息月报",
          content: "及时收集各类",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "日常业务",
          orderNumber: "7",
          score: "3",
          smellItem: "信息月报",
          content: "及时收集各类",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "日常业务",
          orderNumber: "7",
          score: "3",
          smellItem: "信息月报",
          content: "及时收集各类",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "日常业务",
          orderNumber: "7",
          score: "3",
          smellItem: "信息月报",
          content: "及时收集各类",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "日常业务",
          orderNumber: "7",
          score: "3",
          smellItem: "信息月报",
          content: "及时收集各类",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
        {
          bigItem: "日常业务",
          orderNumber: "7",
          score: "3",
          smellItem: "信息月报",
          content: "及时收集各类",
          scoreStandard: "未在规定时间内报备，一起扣0.5分",
          checkInfo:
            "1、有异常情况2、异常情况 3、疫情防控不当4、车辆未规范停靠",
          decodeScore: "4",
          remark: "无",
        },
      ],
    };
  },
  methods: {
    // 初始化
    async init(type, item) {
      this.dioType = type;
      this.dioTit = "考核评分详情";
      // this.inform = {...item}
      this.isDio = true;
      let d = new Map();
      let value = {
        starIndex: 0,
        endIndex: 0,
        colspan: 1,
      };
      // 排序
      this.tableData.forEach((item, index) => {
        if (d.has(item.bigItem)) {
          d.get(item.bigItem).push(item);
        } else {
          d.set(item.bigItem, [item]);
        }
      });
      this.tableData = this.mpaToArray(d);
      d.clear();
      this.tableData.forEach((item, index) => {
        if (d.has(item.bigItem)) {
          value.endIndex++;
          value.colspan++;
        } else {
          value = {
            starIndex: index,
            endIndex: index,
            colspan: 1,
          };
          d.set(item.bigItem, value);
        }
        tableColSpan.set(index, value);
      });
    },

    jump() {
      const navList = JSON.parse(localStorage.getItem("navList"));
      let shiftItem; // 路由
      let isExist = false;
      let breadcrumbActive;
      navList.forEach((item) => {
        if (item.url == "/jwzl/examine-deduct-points") {
          shiftItem = item;
        }
      });
      if (!shiftItem) return;
      localStorage.setItem("navMenuActive", shiftItem.id);
      const breadcrumbListJSON = localStorage.getItem("breadcrumbList");
      if (breadcrumbListJSON) {
        const breadcrumbList = JSON.parse(
          localStorage.getItem("breadcrumbList")
        );
        isExist = breadcrumbList.find((item, index) => {
          if (item.id == shiftItem.id) {
            breadcrumbActive = index;
            return true;
          }
        });
        if (!isExist) {
          if (breadcrumbList.length >= 10) {
            breadcrumbList.shift();
          }
          breadcrumbList.push(shiftItem);
          breadcrumbActive = breadcrumbList.length - 1;
          localStorage.setItem(
            "breadcrumbList",
            JSON.stringify(breadcrumbList)
          );
          localStorage.setItem("breadcrumbActive", breadcrumbActive);
        } else {
          localStorage.setItem("breadcrumbActive", breadcrumbActive);
        }
      } else {
        localStorage.setItem("breadcrumbList", JSON.stringify([shiftItem]));
        localStorage.setItem("breadcrumbActive", 0);
      }
      this.$router.push({ path: shiftItem.url });
      this.isDio = false;
    },

    // 头部样式   前2列合并
    cells({ row, column, rowIndex, columnIndex }) {
      if (row[0].level == 1) {
        row[0].colSpan = 0;
        row[1].colSpan = 2;
        if (columnIndex === 0) {
          return { display: "none" };
        }
      }
      //改变当前行样式
      // return 'font-size:14px;color:#fff;background:#4F81BD;';
    },

    oneStyle({ row, column, rowIndex, columnIndex }) {
      if (columnIndex == 0 || columnIndex == 1) {
        return "font-weight:bold;color:black;";
      }
    },

    arraySpanMethod({ row, column, rowIndex, columnIndex }) {
      if (columnIndex === 0) {
        if (rowIndex === tableColSpan.get(rowIndex).starIndex) {
          if (!row.smellItem) {
            return [tableColSpan.get(rowIndex).colspan, 2];
          } else {
            return [tableColSpan.get(rowIndex).colspan, 1];
          }
        } else if (rowIndex <= tableColSpan.get(rowIndex).endIndex) {
          return [0, 0];
        }
      }

      if (columnIndex === 1) {
        if (!row.smellItem) {
          return [0, 0];
        }
      }

      return [1, 1];
    },

    mpaToArray(d) {
      let result = [];
      for (const [index, item] of d.entries()) {
        result = [...result, ...item];
      }
      return result;
    },

    // 导出PDF
    handleExportPDF() {
      getPdf("广东省监狱系统指挥中心日常工作考核评分表", "pdfHtml");
    },

    // 关闭弹窗
    closeDio() {
      this.isDio = false;
      this.inform = {
        mettingTime: "", // 会议时间
        unit: "", // 单位id
        orgName: "",
        bigItem: "",
        smallItem: "",
        content: "",
        scoreStandard: "",
        score: "",
        checkInfo: "",
        deductPoint: "",
        remarks: "",
        examineOpinion: "",
        feedbackContent: "",
      };
    },
  },
};
</script>

<style lang="scss">
.tableHead {
  color: black !important;
  font-weight: bold !important;
  font-size: 18px;
}
</style>
<style lang="scss" scoped>
.p65 {
  padding: 0px 65px 10px 65px;
}
::v-deep .custom-dialog {
  .el-dialog__body {
    padding: 0px 0px 0px 0px;
    overflow-x: hidden;
    overflow-y: auto;
  }
}

.body-container {
  height: 500px;
}

.atooltip {
  background: #ffffff !important;
  border: 1px solid #000000;
  cursor: pointer;
  font-size: 15px;
}
/deep/ .el-tooltip {
  cursor: pointer;
}

/deep/ .el-table {
  tr:hover > td.el-table__cell {
    background: none;
  }
  th.is-leaf::after {
    display: none;
  }
  th.el-table__cell.is-leaf,
  td.el-table__cell {
    border-color: #333;
  }
  &.el-table--border {
    th.el-table__cell {
      background: #fff;
    }
  }
}
/deep/ .el-table--border {
  border-color: #333;
  th.el-table__cell {
    border-color: #333;
  }
}

.el-table--group::after,
.el-table--border::after {
  background-color: #333;
}

::v-deep .backTop {
  position: relative;
  color: #333;
  top: calc(50% + 170px);
  &:hover {
    color: #2d72fc;
  }
  .el-icon-caret-top {
    display: none;
  }
}
.center_nav {
  position: fixed;
  width: 40px;
  right: 20px;
  top: 50%;
  z-index: 9999;
  .el-button {
    margin: 5px 0;
    font-size: 18px;
  }
  &.detail {
    top: calc(50% + 110px);
  }
}
</style>
