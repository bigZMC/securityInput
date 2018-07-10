<template>
    <div id="securityInput" @click="focusDigit" :style="{'width': length * 30 + 'px'}">
        <i class="sixDigitPassword" v-for="(item, index) in inputValues">
            <i class="fa fa-circle" v-show="'' !== item.value"></i>
        </i>
        <span class="currentDigit" :style="{'left': currentDigitLeft}" v-show="beInputing"></span>
    </div>
</template>

<script>
export default {
    name: 'SecurityInput',
    data() {
        return {
            inputValues: [],
            beInputing: false,
            digitIndex: 0,
            arrayMaxIndex: 0
        }
    },
    props: {
        length: {
            type: Number,
            default: 6
        }
    },
    created() {
        let that = this;
        // 监听focus事件
        document.onclick = function(){
            if('currentDigit' === event.target.className || 'sixDigitPassword' === event.target.className){
                that.beInputing = true;
            }else{
                that.beInputing = false;
            }
        }
        // 监听键盘事件
        document.onkeyup = function(){
            that.digitInput(event.key, event.keyCode);
        }
    },
    mounted() {
        this.inputValues = new Array(this.length).fill('').map(item => {
            return {
                value: ''
            }
        });
        this.arrayMaxIndex = this.length - 1;
    },
    computed: {
        currentDigitLeft() {
            return -1 + 30 * this.digitIndex + 'px';
        }
    },
    methods: {
        focusDigit() {
            // 标识正在进行输入
            this.beInputing = true;
            // 找到当前的输入框索引
            let index = this.inputValues.findIndex(item => '' === item.value);
            // 没有找到未输入的框,代表输入已经完成,到最后一位
            index === -1 && (index = this.arrayMaxIndex);
            this.digitIndex = index;
        },
        digitInput(key, keyCode) {
            // 退格键
            if(8 === keyCode){
                if(0 === this.digitIndex && '' !== this.inputValues[0].value){
                    this.inputValues[0].value = '';
                }else if(this.arrayMaxIndex == this.digitIndex && '' !== this.inputValues[this.arrayMaxIndex].value){
                    this.inputValues[this.digitIndex].value = '';
                }else if(0 < this.digitIndex){
                    this.inputValues[this.digitIndex-- - 1].value = '';
                }
            }else if(!this.allowPass(key)){
                // 空格键和回车键
                return false;
            }else{
                // 如果索引为arrayMaxIndex则是最后一个元素
                if(this.arrayMaxIndex === this.digitIndex && '' === this.inputValues[this.arrayMaxIndex].value){
                    this.inputValues[this.arrayMaxIndex].value = key;
                }else if(this.arrayMaxIndex > this.digitIndex){
                    this.inputValues[this.digitIndex++].value = key;
                }
            }
            this.$emit('securityInput', this.inputValues);
        },
        // 目前只允许数字和字母通过验证
        allowPass(key) {
            let reg = /^[a-zA-Z0-9]$/;
            return reg.test(key);
        }
    }
}
</script>

<style lang="scss" scoped>
$primaryColor: #FF761A;
$digitWidth: 29px;

#securityInput{
    width: 160px;
    cursor: text;
    background: #fff;
    outline: none;
    position: relative;
    padding: 8px 0;
    height: 14px;
    border: 1px solid #ccc;
    border-radius: 2px;
    i.sixDigitPassword{
        float: left;
        display: block;
        padding: 4px 0;
        width: $digitWidth;
        height: 7px;
        line-height: 7px;
        text-align: center;
        border-left: 1px solid #ccc;
        & > i{
            font-size: 8px;
        }
    }
    i.sixDigitPassword:nth-child(1){
        border-left: 0;
    }
    .currentDigit{
        width: $digitWidth;
        position: absolute;
        display: block;
        top: -1px;
        left: -1px;
        height: 30px;
        border: 1px solid $primaryColor;
        border-radius: 2px;
        box-shadow: inset 0 0px 1px $primaryColor, 0 0 8px $primaryColor;
    }
}
</style>
