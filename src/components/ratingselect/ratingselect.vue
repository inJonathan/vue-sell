<template>
    <div class="ratingselect">
        <div class="rating-type border-1px">
            <span @click="select(2, $event)" class="block positive" :class="{'active': selectType === 2}">{{desc.all}} <span class="count">{{ratings.length}}</span></span>
            <span @click="select(0, $event)" class="block positive" :class="{'active': selectType === 0}">{{desc.positive}} <span class="count">{{positive.length}}</span></span>
            <span @click="select(1, $event)" class="block negative" :class="{'active': selectType === 1}">{{desc.negative}} <span class="count">{{negative.length}}</span></span>
        </div>
        <div @click="toggleContent($event)" class="switch" :class="{'on': onlyContent}">
            <span class="icon-check_circle"></span>
            <span class="text">只看有内容的评价</span>
        </div>
    </div>
</template>

<script>
    const POSITIVE = 0;
    const NEGATIVE = 1;
    const ALL = 2;

    export default {
        // 接收从父组件传过来的数据
        props: {
            ratings: {
                type: Array,
                default() {
                    return [];
                }
            },
            selectType: {
                type: Number,
                default: ALL
            },
            onlyContent: {
                type: Boolean,
                default: false
            },
            desc: {
                type: Object,
                // 默认展示的数据
                default() {
                    return {
                        all: '全部',
                        positive: '满意',
                        negative: '不满意'
                    };
                }
            }
        },
        computed: {
            positive() {
                // 对数组进行过滤
                return this.ratings.filter((rating) => {
                    return rating.rateType === POSITIVE;
                });
            },
            negative() {
                // 对数组进行过滤
                return this.ratings.filter((rating) => {
                    return rating.rateType === NEGATIVE;
                });
            }
        },
        methods: {
            select(type, event) {
                // 干掉原生点击事件
                if (!event._constructed) {
                    return;
                }
                this.selectType = type;
                // 派发事件通知父组件数据改变
                // 子组件改变状态，通过事件的方式传递给父组件，父组件重新赋值
                this.$dispatch('ratingtype.select', type);
            },
            toggleContent(event) {
                if (!event._constructed) {
                    return;
                }
                this.onlyContent = !this.onlyContent;
                // 派发事件通知父组件数据改变
                // 子组件改变状态，通过事件的方式传递给父组件，父组件重新赋值
                this.$dispatch('content.toggle', this.onlyContent);
            }
        }
    };
</script>

<style lang="scss" rel="stylesheet/scss">
@import "../../common/sass/mixin.scss";

    .ratingselect {
        .rating-type {
            padding: 18px 0;
            margin: 0 18px;
            @include border-1px(rgba(7, 17, 27, .1));
            font-size: 0;
            .block {
                display: inline-block;
                padding: 8px 12px;
                margin-right: 8px;
                border-radius: 1px;
                line-height: 16px;
                font-size: 12px;
                color: rgb(77, 85, 93);
                &.active {
                    color: #fff;
                }
                .count {
                    font-size: 10px;
                }
                &.positive {
                    background: rgba(0, 160, 200, .2);
                    &.active {
                        background: rgb(0, 160, 200);
                    }
                }
                &.negative {
                    background: rgba(77, 85, 93, .2);
                    &.active {
                        background: rgb(77, 85, 93);
                    }
                }
            }
        }
        .switch {
            padding: 12px 18px;
            line-height: 24px;
            border-bottom: 1px solid rgba(7, 17, 27, .1);
            color: rgb(147, 153, 159);
            font-size: 0;
            &.on {
                .icon-check_circle {
                    color: #00c850;
                }
            }
            .icon-check_circle {
                display: inline-block;
                margin-right: 4px;
                font-size: 24px;
            }
            .text {
                display: inline-block;
                vertical-align: top;
                font-size: 12px;
            }
        }
    }
</style>