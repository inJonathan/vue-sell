<template>
    <div class="cartcontrol">
        <div class="cart-decrease icon-remove_circle_outline" v-show="food.count > 0" @click="decreaseFood"></div>
        <div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
        <div class="cart-add  icon-add_circle" @click="addFood"></div>
    </div>
</template>

<script>
    import Vue from 'vue';

    export default {
        props: {
            food: {
                type: Object
            }
        },
        methods: {
            addFood(event) {
                // 不是自己派生的，而是通过better-scroll派生的事件就return掉
                if (!event._constructed) {
                    return;
                }
                if (!this.food.count) {
                    // 通过Vue.set的方法添加一个属性，变化就能被观测到
                    Vue.set(this.food, 'count', 1);
                } else {
                    this.food.count++;
                }
            },
            decreaseFood(event) {
                // 不是自己派生的，而是通过better-scroll派生的事件就return掉
                if (!event._constructed) {
                    return;
                }
                if (this.food.count) {
                    this.food.count--;
                }
            }
        }
    };
</script>

<style lang="scss" rel="stylesheet/scss">
    .cartcontrol {
        font-size: 0;
        .cart-decrease {
            display: inline-block;
            padding: 6px;
            line-height: 24px;
            font-size: 24px;
            color: rgb(0, 160, 220);
        }
        .cart-count {
            display: inline-block;
            vertical-align: top;
            width: 12px;
            padding-top: 6px;
            line-height: 24px;
            text-align: center;
            font-size: 12px;
            color: rgb(147, 153, 159);
        }
        .cart-add {
            display: inline-block;
            padding: 6px;
            line-height: 24px;
            font-size: 24px;
            color: rgb(0, 160, 220);
        }
    }
</style>