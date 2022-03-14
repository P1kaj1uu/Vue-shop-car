<template>
    <div class="counter">
        <!-- 减1的按钮 -->
        <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
        <!-- 购买的数量 -->
        <span class="number-box">{{num}}</span>
        <!-- 加1的按钮 -->
        <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
    </div>
</template>

<script>
//当组件嵌套时,且相隔较远时可以认为是兄弟组件eventBus
import bus from '../eventBus.js'

export default {
    props: {
        //接收到的商品数量
        num: {
            default: 1,
            type: Number
        },
        //接收商品的id,使用eventBus方案通知App组件根据id更新商品数量
        id: {
            type: Number,
            required: true
        }
    },
    methods: {
        add() {
            const Obj = {id: this.id, value: this.num + 1};
            bus.$emit('share', Obj);
        },
        sub() {
            if(this.num == 1) return;
            const Obj = {id: this.id, value: this.num - 1};
            bus.$emit('share', Obj);
        }
    }
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>