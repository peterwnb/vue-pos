<template>
  <div id="Pos">
    <el-row>
      <el-col :span="8" class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点单">
            <el-table :data="tableData" show-summary border style="width:100%;">
              <el-table-column prop="goodsName" label="商品名称" :resizable="resizable" align="center"></el-table-column>
              <el-table-column width="70" prop="count" label="数量" :resizable="resizable" align="center"></el-table-column>
              <el-table-column width="70" prop="price" label="价格" :resizable="resizable" align="center"></el-table-column>
              <el-table-column width="100" label="操作" :resizable="resizable" align="center">
                <template scope="scope">
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">新增</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="">
              <small>数量：</small>{{totalCount}} &nbsp&nbsp&nbsp&nbsp <small>金额：</small>{{totalMoney}}
            </div>
            <div class="order-btns-div">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="cleanGoods()">清空</el-button>
              <el-button type="success" @click="checkout()">结账</el-button>
            </div>
          </el-tab-pane>

          <el-tab-pane label="挂单">挂单</el-tab-pane>

          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </el-col>

      <el-col :span="16">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="goods-type">
          <el-tabs type="border-card">
            <el-tab-pane label="汉堡">
              <div class="">
                <ul class='cookList'>
                    <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <ul class='cookList'>
                  <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <ul class='cookList'>
                  <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <ul class='cookList'>
                  <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
              </ul>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Pos',
  data () {
    return {
      resizable: false,
      tableData: [],
      oftenGoods:[],
      type0Goods:[],
      type1Goods:[],
      type2Goods:[],
      type3Goods:[],
      totalMoney:0,
      totalCount:0
    }
  },
  created() {
    //拉取常用商品列表
    axios.get('http://jspang.com/DemoApi/oftenGoods.php').then(response=>{
        //console.log(response);
        this.oftenGoods = response.data;
     }).catch(error=>{
         console.log(error);
         alert('网络错误，不能访问');
     });

     //读取分类商品列表
    axios.get('http://jspang.com/DemoApi/typeGoods.php').then(response=>{
       //console.log(response);
       //this.oftenGoods=response.data;
       this.type0Goods=response.data[0];
       this.type1Goods=response.data[1];
       this.type2Goods=response.data[2];
       this.type3Goods=response.data[3];
    }).catch(error=>{
        console.log(error);
        alert('网络错误，不能访问');
    })
  },
  mounted() {
    var orderHeight = document.body.clientHeight;
    console.log(orderHeight);
    document.getElementById('order-list').style.height = orderHeight+'px';
  },
  methods: {
    addOrderList(goods) {
      //判断商品是否已经存在于订单列表中
      let isHave = false;
      for(let i = 0;i<this.tableData.length;i++){
        if(this.tableData[i].goodsId == goods.goodsId){
          isHave = true;
          break;
        }
      }
      //根据判断的值编写业务逻辑
      if(isHave){
        //改变列表中商品的数量
        let arr = this.tableData.filter(o=>o.goodsId == goods.goodsId);
        arr[0].count++;
      } else {
        //直接新增商品到列表
        let newGoods = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
        this.tableData.push(newGoods);
      }
      this.getAllMoney();

    },
    delSingleGoods(goods){
      //删除单独的一个商品
      this.tableData = this.tableData.filter(o=>o.goodsId != goods.goodsId);

      this.getAllMoney();
    },
    cleanGoods(){
      this.tableData = [];
      this.totalMoney = 0;
      this.totalCount = 0;
    },
    getAllMoney(){
      this.totalMoney = 0;
      this.totalCount = 0;
      if(this.tableData){
        //计算汇总金额和数量
        this.tableData.forEach((element)=>{
          this.totalCount += element.count;
          this.totalMoney += (element.price * element.count);
        })
      }
    },
    checkout(){
      //模拟结账
      if(this.totalCount != 0){
        //如果有数据
        this.cleanGoods();
        //提示已结账
        this.$message({
          message: '结账成功，感谢你又为店里出了一份力！',
          type:'success'
        });
      }
    }
  }
}
</script>

<style scoped>
  .pos-order{
    background-color: #fff;
    border-right: 1px solid #C0CCDA;
    height: 100%;
  }

  .order-btns-div{
    margin-top: 10px;
  }

  .title{
    text-align: left;
    height: 20px;
    border-bottom: 1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding: 10px;
  }

  .often-goods-list ul li{
    list-style: none;
    float: left;
    border: 1px solid #E5E9F2;
    padding: 10px;
    margin: 10px;
    background-color: #FFF;
    cursor: pointer;
    font-size: 16px;
  }

  .often-goods-list ul li:hover{
    background-color: #E4E1E1;
  }

  .o-price{
    color: #58B7FF;
  }

  .goods-type{
    clear: both;
    padding-top: 20px;
  }

  .cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
       cursor: pointer;

   }
   .cookList li span{

        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 16px;
       padding-left: 10px;
       color:brown;

   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
</style>
