<template>
  <div class="order_item">
    <van-row class="header"> 
      <van-col span="12">订单编号: {{data.id}}</van-col>
      <van-col span="12" class="status">{{data.status}}</van-col>
    </van-row>
    <van-row>
      <van-col :span="24" :offset="1">
        <div v-if="data.waiter!=null">
          员工姓名：{{data.waiter.realname}}</div>
        <div v-if="data.waiter!=null">
          员工联系方式：{{data.waiter.telephone}}</div>
        <div>服务内容:
          <span v-for="line in data.orderLines" :key="line.id">
            {{line.product.name}}
          </span>
        </div>
        <div>服务时间：{{data.orderTime | datefmt}}</div>
        <div>服务地点：
          {{data.address.province}}
          {{data.address.city}}
          {{data.address.area}}
          {{data.address.address}}
        </div>
      </van-col>
    </van-row>
    <div class="text-right">
      合计￥ {{data.total}}
    </div>

    <div class="text-right" v-if="data.status==='待确认'"><br>
        <van-button size="small" type="primary" plain 
        @click="finishHandler(data.id)">确认</van-button>
    </div>
  </div>
</template>
<script>
import {get} from '../http/axios';
export default {
  props:{
    data:{type:Object}
  },
  methods:{
      finishHandler(id){
        let url = "/order/confirmOrder";
        let params={
          orderId:id
        }
        get(url,params).then(()=>{
          this.$router.go(0);
          this.$toast("确认成功");
        })
      }
  }
}
</script>
<style scoped>
.order_item {
  margin: .5em 1em;
  padding: .5em;
  border-radius: 5px;
  background: #ffffff;
}
.order_item .header {
  line-height: 2.5em;
  font-size: 14px;
}
.order_item .header .status {
  text-align: right;
  font-size: 12px;
  color: #fd621f;
}
.order_item img {
  width: 100%;
  border-radius: 3px;
}


</style>