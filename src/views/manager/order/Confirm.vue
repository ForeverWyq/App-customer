<template>
    <briup-fulllayout title="订单确认" style="paddingTop:1em">
        <div style="paddingLeft:2em;paddingRight:2em;paddingTop:1em">
        服务地址:
        <van-dropdown-menu>
            <van-dropdown-item v-model="addressId" :options="options" />
        </van-dropdown-menu>
        订单详情:
        <div style="padding:0 2em">
            <p>服务名称:{{$route.query.name}}</p>
            <p>服务简介:{{$route.query.description}}</p>
            <p>服务价格:{{$route.query.price}}</p>
            <p>服务数量:1</p>
            <p>服务小计:{{$route.query.price * 1}}</p>
        </div>
        </div>
        <div class="btn" @click="submitHandler()">提交订单</div>
        <!-- <div style="position:fixed;botton:0;width:100%">
            <van-button block type="primary" @click="submitHandler">提交订单</van-button>
        </div> -->
    </briup-fulllayout>
</template>
<script>
import { mapState } from 'vuex';
import { get,post_obj_array } from '../../../http/axios'
export default {
    data(){
        return{
            addresses:[],
            addressId:0,  //服务地址id
            orderLines:[]
        }
    },
    created(){
        this.loadAddress();
        // 初始化订单项(将我们的产品放入购物车)
        let orderLine ={
            number:1,
            price:this.$route.query.price,
            productId:this.$route.query.id
        };
        this.orderLines.push(orderLine);
    },
    computed:{
        // 映射info信息
        ...mapState("user",["info"]),
        
        options:function(){
            // 将address数据计算为一个新的数组返回
            return this.addresses.map(item=>{
                return{
                    text:item.province+" "+item.city+" "+item.area+
                    " "+item.address,
                    value:item.id
                }
            })
        }
    },
    methods:{
        loadAddress(){
            let id=this.info.id;
            let url="/address/findByCustomerId?id="+id;
            get(url).then((response)=>{
                // 将查询到的地址信息绑定到address这个变量中
                this.addresses=response.data;
                // 将查询到的地址列表中的第一个地址id赋值给form中addressId，表示默认地址
                this.addressId = this.addresses[0].id;
            })
        },
        submitHandler(){
            let url="/order/save";
            let data = {
                customerId:this.info.id,
                addressId:this.addressId,
                orderLines:this.orderLines
            }
            // console.log(data);
            post_obj_array(url,data).then((response)=>{
                this.$toast("提交成功");
                this.$router.push("/manager/order")
            })
        }
    }
}
</script>
<style scoped>
    .btn {
        width: 90%;
        margin: 1em auto;
        line-height: 3em;
        text-align: center;
        border: 1px solid #07C160;
        background: #07C160;
        color: #ffffff;
        border-radius: 1.5em;
    }
</style>