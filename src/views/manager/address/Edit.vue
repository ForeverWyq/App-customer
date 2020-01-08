<template>
    <briup-fulllayout title="新增地址" style="paddingTop:1em">
        <div>
            <van-cell-group>
                <van-field type="number" v-model="form.telephone" placeholder="手机号"/>
            </van-cell-group>
            <van-cell-group>
                <van-field v-model="form.province" placeholder="省"/>
            </van-cell-group>
            <van-cell-group>
                <van-field v-model="form.city" placeholder="市"/>
            </van-cell-group>
            <van-cell-group>
                <van-field v-model="form.area" placeholder="区"/>
            </van-cell-group>
            <van-cell-group>
                <van-field v-model="form.address" placeholder="详细地址"/>
            </van-cell-group>
            <div class="btn" @click="submitHandler()">保存</div>
        </div>
    </briup-fulllayout>
</template>
<script>
import { mapState } from 'vuex';
import { post } from '../../../http/axios'
export default {
    data(){
        return{
            form:{}
        }
    },
    computed:{
        ...mapState('user',['info'])
    },
    methods:{
        submitHandler(){
            let url="/address/saveOrUpdate"
            this.form.customerId = this.info.id;
            post(url,this.form).then((response)=>{
                // 返回上一个页面
                this.$router.go(-1);
                this.$toast.success(response.message)
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