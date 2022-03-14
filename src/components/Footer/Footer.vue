<template>
    <div class="footer-container">
        <!-- 左侧的全选 -->
        <div class="custom-control custom-checked">
            <input type="checkbox" class="custom-control-input" @change="fullChange" id="all" :checked="isFull">
            <label class="custom-control-label" for="all">全选</label>
        </div>
        <!-- 中间的合计 -->
        <div>
            <span>合计：</span>
            <span class="total-price">￥{{ zongjia.toFixed(2) }}</span>
        </div>
        <!-- 右边的结算 -->
        <button type="button" class="btn btn-primary btn-settle">结算{{zongshu}}</button>
    </div>
</template>

<script>
//Footer组件是App组件的子组件,要获取父组件传递过来的数据使用自定义属性

export default {
    props: {
        isFull: {
            default: true,
            type: Boolean
        },
        zongjia: {
            default: 0,
            type: Number
        },
        zongshu: {
            default: 0,
            type: Number
        }
    },
    methods: {
        //向父组件中传递点击全选后的数据
        //监听到了全选的状态变化
        fullChange(e) {
            this.$emit('full-change', e.target.checked);
        }
    }
}
</script>

<style lang="less" scoped>
.footer-container {
  font-size: 12px;
  height: 50px;
  width: 100%;
  border-top: 1px solid #efefef;
  position: fixed;
  bottom: 0;
  background-color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
}

.custom-checkbox {
  display: flex;
  align-items: center;
}

#cbFull {
  margin-right: 5px;
}

.btn-settle {
  height: 80%;
  min-width: 110px;
  border-radius: 25px;
  font-size: 12px;
}

.total-price {
  font-weight: bold;
  font-size: 14px;
  color: red;
}
</style>