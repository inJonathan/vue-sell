<template>
    <div class="goods">
        <div class="menu-wrapper">
            <ul>
                <li v-for="item in goods" class="menu-item">
                    <span class="text border-1px">
            	        <span v-show="item.type > 0" class="icon" :class="classMap[item.type]">{{item.name}}</span>
                    </span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper"></div>
    </div>
</template>

<script>
    const ERR_OK = 0;

    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
            return {
                goods: []
            };
        },
        created() {
            this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
            this.$http.get('api/goods').then((response) => {
                response = response.body;
                if(response.errno === ERR_OK) {
                    this.goods = response.data;
                }
            });
        }
    };
</script>

<style lang="scss" rel="stylesheet/scss">
    @import "../../common/sass/mixin";

    .goods {
        display: flex;
        position: absolute;
        top: 174px;
        bottom: 46px;
        width: 100%;
        overflow: hidden;
        .menu-wrapper {
            flex: 0 0 80px;
            width: 80px;
            background-color: #f3f5f7;
            .menu-item {
                display: table;
                width: 56px;
                height: 54px;
                line-height: 14px;
                .icon {
                    display: inline-block;
                    vertical-align: middle;
                    width: 12px;
                    height: 12px;
                    margin-right: 2px;
                    background-size: 100%;
                    background-repeat: no-repeat;
                    &.decrease {
                        @include bg-image('decrease_3');
                    }
                    &.discount {
                        @include bg-image('discount_3');
                    }
                    &.guarantee {
                        @include bg-image('guarantee_3');
                    }
                    &.invoice {
                        @include bg-image('invoice_3');
                    }
                    &.special {
                        @include bg-image('special_3');
                    }
                }
                .text {
                    display: table-cell;
                    width: 56px;
                    vertical-align: center;
                    font-size: 12px;
                    @include border-1px(rgba(7,17,27,.1));
                }
            }
        }
        .foods-wrapper {
            flex: 1;
        }
    }
</style>