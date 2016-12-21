<template>
    <div class="shopcart">
        <div class="content">
            <div class="content-left" :class="{'highlight': totalPrice > 0}">
                <div class="logo-wrapper">
                    <div class="logo">
                        <i class="icon-shopping_cart"></i>
                    </div>
                    <div class="num" v-show="totalPrice > 0">{{totalCount}}</div>
                </div>
                <div class="price"> ¥ {{totalPrice}} 元</div>
                <div class="desc">另需配送费 ¥ {{deliveryPrice}} 元</div>
            </div>
            <div class="content-right">
                <div class="pay" :class="{'enough': totalPrice >= minPrice}">{{payDesc}}</div>
            </div>
        </div>
        <div class="ball-container">
            <div transition="drop" v-for="ball in balls" v-show="ball.show" class="ball">
                <div class="inner inner-hook"></div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            selectFoods: {
                type: Array,
                default() {
                    return [
                        {
                            'price': 15,
                            'count': 1
                        }
                    ];
                }
            },
            deliveryPrice: {
                type: Number,
                default: 0
            },
            minPrice: {
                type: Number,
                default: 0
            }
        },
        data() {
            return {
                balls: [
                    {
                        show: false
                    },
                    {
                        show: false
                    },
                    {
                        show: false
                    },
                    {
                        show: false
                    },
                    {
                        show: false
                    }
                ],
                dropBalls: []
            };
        },
        computed: {
            totalPrice() {
                let total = 0;
                this.selectFoods.forEach((food) => {
                    total += food.price * food.count;
                });
                return total;
            },
            totalCount() {
                let count = 0;
                this.selectFoods.forEach((food) => {
                    count += food.count;
                });
                return count;
            },
            payDesc() {
                if (this.totalPrice === 0) {
                    return ` ¥ ${this.minPrice} 元起送`;
                } else if (this.totalPrice < this.minPrice) {
                    let diff = this.minPrice - this.totalPrice;
                    return `还差 ${diff} 元起送`;
                } else {
                    return '去结算';
                }
            }
        },
        methods: {
            drop(el) {
                for (let i = 0; i < this.balls.length; i++) {
                    let ball = this.balls[i];
                    if (!ball.show) {
                        ball.show = true;
                        ball.el = el;
                        this.dropBalls.push(ball);
                        return;
                    }
                }
            }
        },
        transitions: {
            drop: {
                beforeEnter(el) {
                    // 拿到所有show为true的小球
                    let count = this.balls.length;
                    while (count--) {
                        let ball = this.balls[count];
                        if (ball.show) {
                            let rect = ball.el.getBoundingClientRect();
                            // 获得动画前的偏移量
                            let x = rect.left - 32;
                            let y = -(window.innerHeight - rect.top - 22);
                            el.style.display = '';
                            el.style.webkitTransform = `translate3d(0, ${y}px, 0)`;
                            el.style.transform = `translate3d(0, ${y}px, 0)`;
                            let inner = el.getElementsByClassName('inner-hook')[0];
                            inner.style.webkitTransform = `translate3d(${x}px, 0, 0)`;
                            inner.style.transform = `translate3d(${x}px, 0, 0)`;
                        }
                    }
                },
                enter(el) {
                    /* eslint-disable no-unused-vars */
                    // 手动触发重绘
                    let rf = el.offsetHeight;
                    this.$nextTick(() => {
                        el.style.webkitTransform = 'translate3d(0, 0, 0)';
                        el.style.transform = 'translate3d(0, 0, 0)';
                        let inner = el.getElementsByClassName('inner-hook')[0];
                        inner.style.webkitTransform = 'translate3d(0, 0, 0)';
                        inner.style.transform = 'translate3d(0, 0, 0)';
                    });
                },
                afterEnter(el) {
                    let ball = this.dropBalls.shift();
                    if (ball) {
                        ball.show = false;
                        el.style.display = 'none';
                    }
                }
            }
        }
    };
</script>

<style lang="scss" rel="stylesheet/scss">
    .shopcart {
        position: fixed;
        z-index: 50;
        left: 0;
        bottom: 0;
        width: 100%;
        height: 48px;
        .content {
            display: flex;
            background: #141d27;
            font-size: 0;
            .content-left {
                flex: 1; // 需要自适应
                &.highlight { // 高亮时的状态
                    .logo {
                        background: rgb(0, 160, 220) !important;
                    }
                    .icon-shopping_cart {
                        color: #fff !important;
                    }
                    .price {
                        color: #fff !important;
                    }
                }
                .logo-wrapper {
                    display: inline-block;
                    position: relative;
                    top: -10px;
                    margin: 0 12px;
                    padding: 6px;
                    width: 56px;
                    height: 56px;
                    box-sizing: border-box;
                    vertical-align: top;
                    border-radius: 50%;
                    background: #141d27;
                    .logo {
                        width: 100%;
                        height: 100%;
                        border-radius: 50%;
                        text-align: center;
                        background: #2b343c;
                        .icon-shopping_cart {
                            line-height: 44px;
                            color: #80858a;
                            font-size: 24px;
                        }
                    }
                    .num {
                        position: absolute;
                        top: 0;
                        right: 0;
                        padding: 4px 8px;
                        border-radius: 16px;
                        font-size: 9px;
                        font-weight: 700;
                        color: #fff;
                        background: rgb(240, 20, 20);
                        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, .4);
                    }
                }
                .price {
                    display: inline-block;
                    vertical-align: top;
                    line-height: 24px;
                    margin-top: 12px;
                    padding-right: 12px;
                    box-sizing: border-box;
                    border-right: 1px solid rgba(255, 255, 255, .1);
                    font-size: 16px;
                    font-weight: 700px;
                    color: rgba(255, 255, 255, .4);
                }
                .desc {
                    display: inline-block;
                    vertical-align: top;
                    line-height: 24px;
                    margin: 12px 0 0 12px;
                    font-size: 10px;
                    color: rgba(255, 255, 255, .4);
                }
            }
            .content-right {
                flex: 0 0 105px;
                width: 105px;
                .pay {
                    height:48px;
                    line-height: 48px;
                    text-align: center;
                    font-size: 12px;
                    font-weight: 700;
                    color: rgba(255, 255, 255, .4);
                    background: #2b333b;
                    &.enough {
                        color: #fff;
                        background: #00b43c;
                    }
                }
            }
        }
        .ball-container {
            .ball {
                z-index: 200;
                position: fixed;
                left: 32px;
                bottom: 22px;
                &.drop-transition {
                    transition: all .4s cubic-bezier(.49, -.29, .75, .41);
                    .inner {
                        width: 16px;
                        height: 16px;
                        border-radius: 50%;
                        background: rgb(0, 160, 220);
                        transition: all .4s linear;
                    }
                }
            }
        }
    }
</style>