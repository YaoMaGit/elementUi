<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span='7' class="order-bor" id="order-list">
          <!-- @tab-click="handleClick" -->
          <el-tabs v-model="activeName" >
            <el-tab-pane label="结账" name="first">
               <el-table border 
                  ref="multipleTable"
                  :data="tableData"
                  tooltip-effect="dark"
                  style="width: 100%" >
                  <el-table-column
                    prop="goodsName"
                    label="商品名称"
                    width="120">
                  </el-table-column>
                    <el-table-column
                    prop="count"
                    label="数量"
                    sortable
                    show-overflow-tooltip>
                  </el-table-column>
                  <el-table-column
                    prop="price"
                    label="金额"
                    sortable
                    width="120">
                  </el-table-column>
                
                      <el-table-column  label="操作" width="100" fixed="right">
                          <template scope="scope">
                              <el-button type="text" size="small" @click="deleteRow(scope.$index, tableData)">删除</el-button>
                              <el-button type="text" size="small" @click="add_tableDataClk(tableData[scope.$index])">增加</el-button>
                          </template>
                      </el-table-column>
                </el-table>
                <div class="count_div">
                  <small>数量：</small><b>{{goodsnum}}</b>
                  <small>合计：</small><b>{{goodsmoney}}￥</b>
                </div>
                <div class="btn_div">
                  <el-button type="primary">挂单</el-button>
                  <el-button type="warning">结账</el-button>
                  <el-button type="danger">删除</el-button>
                </div>
            </el-tab-pane>
            <el-tab-pane label="挂单" name="second">配置管理</el-tab-pane>
            <el-tab-pane label="外卖" name="third">角色管理</el-tab-pane>
          </el-tabs>

        </el-col>
        <!--商品展示-->
        <el-col :span="17">
          <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">
              <ul>
                <li v-for="(item,index) in oftenGoods" :key="index" @click="add_tableDataClk(item)">
                  <span>{{item.goodsName}}</span>
                  <span class="o-price">￥{{item.price}}元</span>
                </li>
              </ul>
            </div>
          </div>

          <div class="goods-type">
              <el-tabs>
                  <el-tab-pane label="汉堡">
                      <ul class='cookList'>
                          <li v-for="(item,index) in type0Goods" @click="add_tableDataClk(item)" :key="index">
                              <span class="foodImg">
                                <img src="https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2182134899,3326570472&fm=27&gp=0.jpg" width="100%">
                              </span>
                              <span class="foodName">{{item.goodsName}}</span>
                              <span class="foodPrice">￥{{item.price}}元</span>
                          </li>
                      </ul>
                  </el-tab-pane>
                      <el-tab-pane label="小食">
                       <ul class='cookList'>
                          <li  v-for="(item,index) in type1Goods" @click="add_tableDataClk(item)" :key="index">
                              <span class="foodImg">
                                <img src="https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2182134899,3326570472&fm=27&gp=0.jpg" width="100%">
                              </span>
                              <span class="foodName">{{item.goodsName}}</span>
                              <span class="foodPrice">￥{{item.price}}元</span>
                          </li>
                      </ul>
                  </el-tab-pane>
                  <el-tab-pane label="饮料">
                      <ul class='cookList'>
                          <li v-for="(item,index) in type2Goods" @click="add_tableDataClk(item)" :key="index">
                              <span class="foodImg">
                                <img src="https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2182134899,3326570472&fm=27&gp=0.jpg" width="100%">
                              </span>
                              <span class="foodName">{{item.goodsName}}</span>
                              <span class="foodPrice">￥{{item.price}}元</span>
                          </li>
                      </ul>
                  </el-tab-pane>
                  <el-tab-pane label="套餐">
                       <ul class='cookList'>
                          <li v-for="(item,index) in type3Goods" @click="add_tableDataClk(item)" :key="index">
                              <span class="foodImg">
                                <img src="https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2182134899,3326570472&fm=27&gp=0.jpg" width="100%">
                              </span>
                              <span class="foodName">{{item.goodsName}}</span>
                              <span class="foodPrice">￥{{item.price}}元</span>
                          </li>
                      </ul>
                  </el-tab-pane>
              </el-tabs>
          </div>


        </el-col>
      </el-row>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import { log } from "util";
export default {
  name: "Pos",
  data() {
    return {
      activeName: "first",
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      multipleSelection: [],
      goodsnum: 0,
      goodsmoney: 0
    };
  },
  mounted: function() {
    var orderHeight = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderHeight + "px";
  },
  created() {
    axios
      .get(
        "https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods"
      )
      .then(response => {
        console.log(response);
        this.oftenGoods = response.data;
      })
      .catch(error => {
        console.log(error);
        alert("网络错误，不能访问");
      });

    //读取分类商品列表
    axios
      .get(
        "https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods"
      )
      .then(response => {
        console.log(response);
        //this.oftenGoods=response.data;
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
        this.type3Goods = response.data[3];
      })
      .catch(error => {
        console.log(error);
        alert("网络错误，不能访问");
      });
  },
  methods: {
    add_tableDataClk: function(item) {
      let _this = this;
      _this.goodsnum = 0;
      _this.goodsmoney = 0;
      console.log(item);
      let haveBool = false;
      for (const key in _this.tableData) {
        if (_this.tableData[key].goodsId == item.goodsId) {
          haveBool = true;
        }
      }

      if (haveBool == false) {
        _this.tableData.push({
          goodsId: item.goodsId,
          goodsName: item.goodsName,
          price: item.price,
          count: 1
        });
      }

      if (haveBool == true) {
        for (const key in _this.tableData) {
          if (_this.tableData[key].goodsId == item.goodsId) {
            ++_this.tableData[key].count;
          }
        }
      }

      for (const key in _this.tableData) {
        _this.goodsnum += _this.tableData[key].count;
        _this.goodsmoney +=
          _this.tableData[key].price * _this.tableData[key].count;
      }
    },
    add_tableDataClk2: function(index, data) {
      console.log(index);
      console.log(data);
    },
    deleteRow: function(index, rows) {
      let _this = this;
      _this.goodsnum = 0;
      _this.goodsmoney = 0;
      // rows.splice(index, 1);
      console.log(index);
      console.log(_this.tableData[index].goodsName);
      for (const key in _this.tableData) {
        if (rows[index].goodsId == _this.tableData[key].goodsId) {
          _this.tableData.splice(key,1)
          
        }
      }
      for (const key in _this.tableData) {
        _this.goodsnum += _this.tableData[key].count;
        _this.goodsmoney +=_this.tableData[key].price * _this.tableData[key].count;
      }
    },
    getSummaries(param) {
      console.log(param);
      // const { columns, data } = param;
      // const sums = [];
      // columns.forEach((column, index) => {
      //   if (index === 0) {
      //     sums[index] = '总价';
      //     return;
      //   }
      //   const values = data.map(item => Number(item[column.property]));
      //   if (!values.every(value => isNaN(value))) {
      //     sums[index] = values.reduce((prev, curr) => {
      //       const value = Number(curr);
      //       if (!isNaN(value)) {
      //         return prev + curr;
      //       } else {
      //         return prev;
      //       }
      //     }, 0);
      //     sums[index] += ' 元';
      //   } else {
      //     sums[index] = 'N/A';
      //   }
      // });
      // return sums;
    }
  }
};
</script>
<style scoped>
.order-bor {
  background: #f9fafc;
  border: 1px solid #cccccc;
}
.btn_div {
  width: 100%;
  text-align: center;
  margin-top: 20px;
}

.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
}
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 5px;
  background-color: #fff;
}
.o-price {
  color: #58b7ff;
}
.goods-type {
  clear: both;
}

.cookList li {
  list-style: none;
  width: 20%;
  border: 1px solid #e5e9f2;
  height: auto;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 60px;
}
.foodName {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.count_div {
  width: 100%;
  text-align: center;
}
</style>
