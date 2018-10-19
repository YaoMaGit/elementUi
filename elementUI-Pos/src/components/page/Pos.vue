<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span='7' class="order-bor" id="order-list">
          <!-- @tab-click="handleClick" -->
          <el-tabs v-model="activeName" >
            <el-tab-pane label="结账" name="first">
               <el-table
                  ref="multipleTable"
                  :data="tableData"
                  tooltip-effect="dark"
                  style="width: 100%"
                  show-summary
                  @selection-change="handleSelectionChange">
                  <el-table-column
                    prop="goodsName"
                    label="商品名称"
                    width="120">
                  </el-table-column>
                  <el-table-column
                    prop="price"
                    label="金额"
                    sortable
                    width="120">
                  </el-table-column>
                  <el-table-column
                    prop="count"
                    label="数量"
                    sortable
                    show-overflow-tooltip>
                  </el-table-column>
                      <el-table-column  label="操作" width="100" fixed="right">
                          <template scope="scope">
                              <el-button type="text" size="small">删除</el-button>
                              <el-button type="text" size="small">增加</el-button>
                          </template>
                      </el-table-column>
                </el-table>
            </el-tab-pane>
            <el-tab-pane label="挂单" name="second">配置管理</el-tab-pane>
            <el-tab-pane label="外卖" name="third">角色管理</el-tab-pane>
          </el-tabs>
          <el-row>

            <el-button type="primary">挂单</el-button>
            <el-button type="warning">结账</el-button>
            <el-button type="danger">删除</el-button>
          </el-row>

        </el-col>
        <!--商品展示-->
        <el-col :span="17">
          我是产品栏
        </el-col>
      </el-row>
    </div>
  </div>
</template>
<script>
export default {
  name: "Pos",
  data() {
    return {
      activeName: "first",
      tableData: [
        {
          goodsName: "可口可乐",
          price: 8,
          count: 1
        },
        {
          goodsName: "香辣鸡腿堡",
          price: 15,
          count: 1
        },
        {
          goodsName: "爱心薯条",
          price: 8,
          count: 1
        },
        {
          goodsName: "甜筒",
          price: 8,
          count: 1
        }
      ],
      multipleSelection: []
    };
  },
  mounted: function() {
    var orderHeight = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderHeight + "px";
  },
  methods: {
    toggleSelection(rows) {
      if (rows) {
        rows.forEach(row => {
          this.$refs.multipleTable.toggleRowSelection(row);
        });
      } else {
        this.$refs.multipleTable.clearSelection();
      }
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
    }
  }
};
</script>
<style scoped>
.order-bor {
  background: #f9fafc;
  border: 1px solid #cccccc;
}
</style>
