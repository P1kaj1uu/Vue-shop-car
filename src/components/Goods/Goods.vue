<template>
    <div class="goods-container">
        <!-- 左侧图片 -->
        <div class="thumb">
            <div class="custom-control custom-checkbox">
                <!-- 复选框 -->
                <input @change="stateChange" type="checkbox" name="custom-control-input" :id="'cb' + id" :checked="state">
                <label :for="'cb' + id" class="custom-control-label">
                    <!-- 商品的缩略图 -->
                    <img :src="photo" alt="">
                </label>
            </div>
        </div>
        <!-- 右侧信息区域 -->
        <div class="goods-info">
            <!-- 商品标题 -->
            <h6 class="goods-title">{{ title }}</h6>
            <div class="goods-info-bottom">
                <!-- 商品价格 -->
                <span class="goods-price">￥{{ price }}</span>
                <!-- 商品数量 -->
                <!-- <Counter :num="count" :id="id"></Counter> -->
                <!-- 采用插槽的形式 -->
                <slot></slot>
            </div>
        </div>
    </div>
</template>

<script>
import Counter from '../Counter/Counter.vue'

export default {
    components: {
        Counter
    },
    //Goods是App的子组件,要拿到父组件传递过来的值用自定义属性
    props: {
        //商品的id,子组件中商品状态变化时,通过子传父,根据id修改状态
        id: {
            required: true,
            type: Number
        },
        //要渲染的商品的标题
        title: {
            default: '',
            type: String
        },
        //要渲染的商品的缩略图
        photo: {
            default: '',
            type: String
        },
        //要渲染的商品的价格
        price: {
            default: 0,
            type: Number
        },
        //要渲染的商品的复选框
        state: {
            default: true,
            type: Boolean
        },
        //要渲染的商品的数量
        count: {
            default: 1,
            type: Number
        }
    },
    methods: {
        //Goods子组件向App父组件中传递数据,用自定义事件
        //监听复选框的变化
        stateChange(e) {
            //形参中的e为子组件向父组件传递过来的数据,格式为{id,value}
            const newState = e.target.checked;
            //复选框状态改变时,通过$emit()触发自定义事件
            this.$emit('state-change', {id: this.id, value: newState});
        }
    }
}
</script>

<style lang="less" scoped>
.goods-container {
+ 
.goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>