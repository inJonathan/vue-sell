<template>
    <div class="star" :class="starType">
        <span v-for="itemClass in itemClasses" :class="itemClass" class="star-item" track-by="$index"></span>
    </div>
</template>

<script>
    const LENGTH = 5;
    const CLS_ON = 'on';
    const CLS_OFF = 'off';
    const CLS_HALF = 'half';

    export default {
        props: {
            size: {
                type: Number
            },
            score: {
                type: Number
            }
        },
        computed: {
            starType() {
                return 'star-' + this.size;
            },
            itemClasses() {
                let result = [];
                let score = Math.floor(this.score * 2) / 2;
                let hasDescimal = score % 1 !== 0; // 是否有余数
                let integer = Math.floor(score); // 取到整数
                for(let i = 0; i < integer; i++) {
                    result.push(CLS_ON);
                }
                if(hasDescimal) {
                    result.push(CLS_HALF);
                }
                while(result.length < LENGTH) {
                    result.push(CLS_OFF);
                }
                return result;
            }
        }
    };
</script>

<style lang="scss" rel="stylesheet/scss">
    @import "../../common/sass/mixin";
    .star {
        font-size: 0;
        .star-item {
            display: inline-block;
            background-repeat: no-repeat;
        }
        &.star-48 {
            .star-item {
                width: 20px;
                height: 20px;
                margin-right: 22px;
                background-size: 100%;
                &.on {
                    @include bg-image('star48_on');
                }
                &.off {
                    @include bg-image('star48_off');
                }
                &.half {
                    @include bg-image('star48_half');
                }
                &:last-child {
                    margin-right: 0;
                }
            }
        }
        &.star-36 {
            .star-item {
                width: 15px;
                height: 15px;
                margin-right: 6px;
                background-size: 100%;
                &.on {
                    @include bg-image('star36_on');
                }
                &.off {
                    @include bg-image('star36_off');
                }
                &.half {
                    @include bg-image('star36_half');
                }
                &:last-child {
                    margin-right: 0;
                }
            }
        }
        &.star-24 {
            .star-item {
                width: 10px;
                height: 10px;
                margin-right: 3px;
                background-size: 100%;
                &.on {
                    @include bg-image('star24_on');
                }
                &.off {
                    @include bg-image('star24_off');
                }
                &.half {
                    @include bg-image('star24_half');
                }
                &:last-child {
                    margin-right: 0;
                }
            }
        }
    }
</style>