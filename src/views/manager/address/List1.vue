<template>
    <briup-fulllayout title="常用地址" style="paddingTop:1em">
        <van-address-list
        v-model="chosenAddressId"
        :list="list"
        default-tag-text="默认"
        @add="toAddressEditHandler"
        @edit="onEdit"
        />
    </briup-fulllayout>
</template>
<script>
import { get } from '../../../http/axios'
import { mapState } from 'vuex'
export default {
    data() {
        return {
            chosenAddressId: '1',
            addresses:[]
        }
    },    
    computed:{
        ...mapState("user",["info"]),

         list:function(){
            // 将address数据计算为一个新的数组返回
            return this.addresses.map(item=>{
                return{
                    id:item.id,
                    name:item.customerId,
                    tel:item.telephone,
                    address:item.province+" "+item.city+" "+item.area+
                    " "+item.address   
                }
            })
        }
    },
    created(){
        this.loadAddress();
    },
    methods: {
        loadAddress(){
            let id=this.info.id;
            let url="/address/findByCustomerId?id="+id;
            get(url).then((response)=>{
                this.addresses=response.data;
            })
        },
        toAddressEditHandler() {
            this.$router.push("/manager/address_edit");
        },
        onEdit(edit) {
            this.$router.push({
                path:"/manager/address_edit",
                // query:edit
            })
        }
    }
}
</script>