<template>
  <div class="pos">
    <div>
        <el-row >
            <el-col :span='7' class="order-list" id='order-list'>
              <el-tabs v-model="activeName" @tab-click="handleClick">
                <el-tab-pane name="first">
                        <span slot="label">点餐</span>
                      <el-table border width="100%" :data='tabledata'>
                        <el-table-column
                          prop="goodsName"
                          label="商品名称">
                        </el-table-column>
                        <el-table-column
                          prop="count"
                          label="数量" width="50">
                        </el-table-column>
                        <el-table-column label="金额" width="70">
                          <template slot-scope="sco2pe">
                          <span>{{sco2pe.row.price}}</span>
                          </template>
                        </el-table-column>
                        <el-table-column
                          label="操作" width="100" fixed="right">
                          <template slot-scope="scope">
                          <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                          <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                          </template>
                        </el-table-column>
                      </el-table>
                      <div class="totalDiv">
                        <small>数量:</small>{{this.totalCount}} &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <small>金额:</small>{{this.totalMoney}}元
                      </div>
                      <div class="pos-btns">
                        <el-button type="warning" >挂单</el-button>
                        <el-button type="danger" @click="delAllGoods()">删除</el-button>
                        <el-button type="success" @click="checkout()" >结账</el-button>
                      </div>
                </el-tab-pane>
                <el-tab-pane name="second"><span slot="label">挂单</span></el-tab-pane>
                <el-tab-pane name="third"><span slot="label">外卖</span></el-tab-pane>
              </el-tabs>
            </el-col>
            <!--商品展示-->
            <el-col :span="17">
             <div class="often-goods">
               <div class="title">常用商品</div>
               <div class="often-goods-list">
                 <ul class="clearfloat">
                   <li id='goods' v-for="goods in oftenGoods" :key='goods.goodsName' @click='addOrderList(goods)'>
                     <span>{{goods.goodsName}}</span>
                     <span class="o-price">￥{{goods.price}}元</span>
                   </li>
                 </ul>
               </div>
               <div class="goods-type">
                 <el-tabs>
                   <el-tab-pane><span slot="label" class="goods-type-btn">汉堡</span>
                      <ul class='cookList clearfloat'>
                          <li v-for="item in typeGoods" :key="item.goodsName">
                              <span class="foodImg"><img src="../../assets/logo.png" width="100%"></span>
                              <span class="foodName">{{item.goodsName}}</span>
                              <span class="foodPrice">￥{{item.price}}元</span>
                          </li>
                      </ul>
                   </el-tab-pane>
                   <el-tab-pane><span slot="label" class="goods-type-btn">小吃</span>2</el-tab-pane>
                   <el-tab-pane><span slot="label" class="goods-type-btn">饮料</span></el-tab-pane>
                   <el-tab-pane><span slot="label" class="goods-type-btn">套餐</span></el-tab-pane>
                 </el-tabs>
               </div>
             </div>
            </el-col>
        </el-row>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'Pos',
  data() {
      return {
        activeName: 'first',
        type0Goods:[],
        type1Goods:[],
        type2Goods:[],
        type3Goods:[],
        typeGoods:[],
        tabledata:[],
        oftenGoods:[],
        totalMoney:0,
        totalCount:0
      };
    },
     methods: {
      handleClick(tab, event) {
        console.log(tab, event);
      },
      //添加订单列表的方法
      addOrderList(goods){
        //console.log(this);
        //每次添加清零
        let isHave = false; //没商品
        for(let i=0;i<this.tabledata.length;i++){
          if(this.tabledata[i].goodsId==goods.goodsId){
            isHave = true;//存在商品
          }
        }
        if(isHave){
          //存在就进行数量添加
          //console.log('存在')
          let arr = this.tabledata.filter(o=>o.goodsId == goods.goodsId);//filter过滤
          arr[0].count++;
          //console.log(arr);
        }else{
          let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
          this.tabledata.push(newGoods);
        }
        this.getAllMoney();
      },
      delSingleGoods(goods){
        console.log(goods);
        this.tabledata = this.tabledata.filter(o=>o.goodsId!=goods.goodsId);
        this.getAllMoney();
      },
      delAllGoods(){
        this.tabledata=[];
        this.getAllMoney();
      },
      getAllMoney(){
        this.totalMoney = 0;
        this.totalCount = 0;
        if(this.tabledata){
          this.tabledata.forEach((element)=>{
          this.totalCount += element.count;
          this.totalMoney += element.price*element.count;
        })
        }
      },
      // 结账
            checkout() {
          if (this.totalCount!=0) {
              this.tableData = [];
              this.totalCount = 0;
              this.totalMoney = 0;
              this.$message({
                  message: '结账成功，感谢你又为店里出了一份力!',
                  type: 'success'
              });
              this.delAllGoods();
          }else{
              this.$message.error('不能空结。老板了解你急切的心情！');
          }
      }
    },
   mounted:function () {
      var orderHeight=document.body.clientHeight;
      document.getElementById("order-list").style.height=orderHeight+'px';
    },
   created:function(){
     axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
      .then(response=>{
         //console.log(response);
         this.oftenGoods=response.data;
      })
      .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
      })

            axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
      .then(response=>{
         this.type0Goods=response.data[0];
         this.type1Goods=response.data[1];
         this.type2Goods=response.data[2];
         this.type3Goods=response.data[3];
         this.typeGoods = this.typeGoods.concat(this.type0Goods,this.type1Goods,this.type2Goods,this.type3Goods);//数组合并
         //console.log(this.typeGoods);

      })
      .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
      })
    }
}
</script>
<style scoped>
.pos{

}
.order-list{
  background-color: #dedede;
  height: 100%;
}
.pos-btns{
  margin-top: 30px;
}
.often-goods{

}
.title{
       height: 19px;
       border-bottom:1px solid #D3DCE6;
       background-color: #F9FAFC;
       padding:10px;
       text-align: left;
}
.often-goods-list{
  padding: 15px;
  background-color: #fff;
  border-bottom: 2px solid #D3DCE6;
}
.often-goods-list ul{
  list-style: none;
  margin:0;
  padding: 0;
}
.often-goods-list ul li{
list-style: none;
      float:left;
      border:1px solid #E5E9F2;
      padding:10px;
      margin:5px;
      background-color:#fff;
}
.often-goods .o-price{
 color:#58B7FF;
}
.goods-tabs{
  
}
.goods-tabs .el-tabs__header{
  
}
.order-list span{
  
}
.cookList-wrap{

}
.goods-type-btn{
  width: 100px;
    display: inline-block;
}
   /*清除浮动代码*/
   .clearfloat:after{display:block;clear:both;content:"";visibility:hidden;height:0}
   .clearfloat{zoom:1}
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
   }
   .cookList li span{
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodImg img{
     display: block;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   .goods-type{
  /* background-color: #EFF2F7; */
   }
</style>
