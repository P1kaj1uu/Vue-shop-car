<template>
    <div class="app-container">
        <!-- 使用导入的组件 -->
        <!-- 头部区域 -->
        <Header title="购物车"></Header>
        <!-- 循环渲染每一个商品信息 -->
        <Goods 
        @state-change="getNewState"
        :id="item.id" 
        :title="item.goods_name" 
        :photo="item.goods_img" 
        :price="item.goods_price" 
        :state="item.goods_state" 
        :count="item.goods_count"
        v-for="item in list" :key="item.id">
        </Goods>
        <!-- 底部区域 -->
        <Footer :isFull="fullState" :zongjia="total" :zongshu="sum" @full-change="getFullState"></Footer>
    </div>
</template>

<script>
//导入axios请求库
import axios from 'axios'

//导入所需要的组件
import Footer from './components/Footer/Footer.vue'
import Goods from './components/Goods/Goods.vue'
import Header from './components/Header/Header.vue'

//导入eventBus
import bus from './components/eventBus.js'

export default {
    //注册导入的组件
    components: {
        Footer,
        Goods,
        Header
    },
    data() {
        return {
            //用来存储购物车的列表数据
            list: [],
        }
    },
    methods: {
        //封装请求列表数据的方法
        async initCarList() {
            const {data: res} = await axios.get('https://www.escook.cn/api/cart')
            if(res.status === 200) {
                this.list = res.list;
            }
        },
        //接收Goods子组件传递过来的数据,更新复选框勾选状态
        getNewState(e) {
            //根据商品id修改复选框最新的状态
            //some()方法是遍历数组,找到满足条件的就结束遍历
            this.list.some(item => {
                if(item.id == e.id) {
                    item.goods_state = e.value;
                    //终止后续的循环
                    return true;
                }
            })
        },
        //接收Footer子组件传递过来的全选按钮的状态
        getFullState(val) {
            //循环商品每一项数据,并将全选按钮的状态赋值给每一项的复选框状态
            this.list.forEach(item => item.goods_state = val);
        }
    },
    created() {
        //调用axios请求获取服务器端的数据
        this.initCarList();
        //接收兄弟组件传递过来的数据
        bus.$on('share', val => {
            this.list.some(item => {
                if(item.id == val.id) {
                    item.goods_count = val.value;
                    return true;
                }
            })
        });
        bus.$on('subCount',);
    },
    computed: {
        //动态计算出全选的状态是true还是false
        fullState() {
            //every()方法遍历数组,数组中每一项都满足条件时则返回true,否则返回false
            return this.list.every(item => item.goods_state === true);
        },
        //动态计算出商品的合计总价,父组件向子组件中传递计算出来的总价
        total() {
            //先过滤出已经勾选的商品,再累加
            //reduce((累加的结果,当前循环项) => {},初始值),其方法是累加器
            return this.list.filter(item => item.goods_state == true)
            .reduce((total, item) => total += item.goods_price * item.goods_count, 0);
        },
        //动态计算出已经勾选商品的总数量
        sum() {
            return this.list.filter(item => item.goods_state == true)
            .reduce((sumCount, item) => sumCount += item.goods_count, 0);
        }
    }
}
</script>

<style lang="less" scoped>
* {
    padding: 0;
    margin: 0;
}
.app-container {
    padding-top: 45px;
    padding-bottom: 50px;
}
</style>
