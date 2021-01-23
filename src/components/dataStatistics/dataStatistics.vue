<template>
  <div class="connect">
    <!-- 面包屑 -->
    <el-breadcrumb separator="/">
      <el-breadcrumb-item>医生自评量表</el-breadcrumb-item>
    </el-breadcrumb>
    <div class="card_box">
      <el-card>
        <div class="searchBox">
          <el-input
            size="small"
            class="searchInput"
            placeholder="请输入手机号/姓名"
            prefix-icon="el-icon-search"
            v-model="input"
            @input="serchIn"
          ></el-input>
        </div>
        <!-- 调用公用表格组件 -->
        <ElTable
          :data="userList"
          :header="tableHeaderBig"
          style="margin-top: 1%"
        >
          <el-table-column
            align="center"
            slot="fixed"
            fixed="right"
            label="录入时间"
            prop="createTime"
          >
            <template slot-scope="scope">
              <div>{{ timesChangeDate(scope.row.createTime) }}</div>
            </template>
          </el-table-column>
          <el-table-column
            align="center"
            slot="fixed"
            fixed="right"
            label="测试情况"
            prop="state"
            :formatter="ifendcaseJck"
          ></el-table-column>
          <el-table-column
            align="center"
            slot="fixed"
            fixed="right"
            label="操作"
          >
            <template slot-scope="scope">
              <el-button
                type="primary"
                size="mini"
                @click.prevent.stop="JumpUserCenter(scope.row)"
                >查看</el-button
              >
            </template>
          </el-table-column>
        </ElTable>
        <!-- 分页 -->
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChangev"
          :current-page="pageNum"
          :page-sizes="[10, 20, 50]"
          :page-size="pageSize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total"
        ></el-pagination>
      </el-card>
    </div>
  </div>
</template>
<script>
import ElTable from "../CommonModule/table";
import { timesChangeDate } from "../../assets/js/util";
export default {
  components: { ElTable },
  data() {
    return {
      timesChangeDate,
      tableHeaderBig: [
        { prop: "orderNo", label: "检测卡号" },
        { prop: "name", label: "姓名" },
        { prop: "phone", label: "手机号" },
      ],
      test: {
        records: [
          {
            id: 1,
            regionNo: "R0001",
            regionName: "鹰潭市供水集团",
            minVOList: [
              {
                minFlow: 6,
                maxFlow: 6,
                avgFlow: 6,
                minDate: "2020-12-14",
              },
              {
                minFlow: null,
                maxFlow: null,
                avgFlow: null,
                minDate: "2020-12-11",
              },
              {
                minFlow: null,
                maxFlow: null,
                avgFlow: null,
                minDate: "2020-12-12",
              },
              {
                minFlow: null,
                maxFlow: null,
                avgFlow: null,
                minDate: "2020-12-13",
              },
              {
                minFlow: null,
                maxFlow: null,
                avgFlow: null,
                minDate: "2020-12-15",
              },
              {
                minFlow: null,
                maxFlow: null,
                avgFlow: null,
                minDate: "2020-12-16",
              },
              {
                minFlow: null,
                maxFlow: null,
                avgFlow: null,
                minDate: "2020-12-17",
              },
              {
                minFlow: null,
                maxFlow: null,
                avgFlow: null,
                minDate: "2020-12-18",
              },
            ],
            maxFlow: 6,
            maxDate: "2020-12-14",
            minFlow: 6,
            minDate: "2020-12-14",
            average: 6,
            nightStartTime: "01",
            nightEndTime: "22",
          },
        ],
      },
      userList: [],
      pageSize: 10,
      pageNum: 1,
      total: 0,
      input: "",
    };
  },
  created() {
    let arrData = this.test.records;
    let data = {};
    for (var item  of arrData) {
      console.log(item);
      data[item.minDate] = item.minFlow;
    }
    console.log(data);

    this.getCardList();
  },
  methods: {
    // 获取检查单列表
    async getCardList() {
      const { data: res } = await this.$http.post("checkList/list", {
        type: 1,
        pageSize: this.pageSize,
        pageNum: this.pageNum,
        name: this.input,
      });
      this.userList = res.rows;
      this.total = res.total;
    },
    JumpUserCenter() {
      this.$router.push("userCenter");
    },
    // 搜索
    serchIn() {
      this.getCardList();
    },
    // 分页
    handleSizeChange(newSize) {
      this.pageSize = newSize;
    },
    handleCurrentChangev(newPage) {
      this.pageNum = newPage;
      this.getCardList();
    },
    // 检测卡类型状态码数字转中文
    ifendcaseJck(val) {
      if (val.state == "1") {
        return "已检测";
      } else if (val.state == "2") {
        return "未检测";
      }
    },
  },
};
</script>
<style scoped>
.connect {
  height: 100%;
}
.card_box {
  width: 100%;
  height: 85%;
  overflow: hidden;
}
.searchBox {
  display: -webkit-flex;
  display: flex;
}
.searchInput {
  width: 25%;
  max-width: 300px;
}
.el-card {
  width: 100%;
  height: 100%;
  overflow: auto;
  -webkit-overflow-scrolling: touch;
}
</style>