<template>
    <div class="cartcontrol">
        <div class="cart-decrease" v-show="food.count > 0" @click.stop.prevent="decreaseFood" transition="move">
            <span class="inner icon-remove_circle_outline"></span>
        </div>
        <div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
        <div class="cart-add  icon-add_circle" @click.stop.prevent="addFood"></div>
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
                // 派发一个事件出去，附带自身的dom节点
                this.$dispatch('cart.add', event.target);
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
            transition: all .4s linear;
            &.move-transition {
                opacity: 1;
                transform: translate3d(0, 0, 0);
                .inner {
                    display: inline-block;
                    line-height: 24px;
                    font-size: 24px;
                    color: rgb(0, 160, 220);
                    transition: all .4s linear;
                    transform: rotate(0);
                }
            }
            &.move-enter, &.move-leave {
                opacity: 0;
                transform: translate3d(24px, 0, 0);
                .inner {
                    transform: rotate(180deg);
                }
            }
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