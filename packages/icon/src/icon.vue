<template>
    <span class="mint-icon" :class="animateClass">
        <i @click="handleClick" class="iconfont mint-icon-i" :style="computeStyle" :class="name">
        </i>
    </span>

</template>
<script>
    /**
     * @noteType component
     * @name Icon
     * @tagName mt-icon
     * @desc 分隔线
     * @wrapClassName mint-icon
     * @html
     * <mt-icon></mt-icon>
     */
    export default {
        name: 'mt-icon',
        methods: {
            handleClick(evt) {
                if(this.changecolor){
                    this.isAddbcColor=this.isAddbcColor?'':this.changecolor;
                }
                if(this.animate){
                    this.animateClass = this.animate;
                    let that=this;
                    let tmier=setInterval(function(){
                      that.animateClass='';
                      clearInterval(tmier);
                    },1000);
                }
                this.$emit('click', evt);
            }
        },
        props: {
            size: String,
            animate: String,
            changecolor: String,
            name: String,
            color: String,
            /**
             * @name 图标样式
             * @fontsize 图标大小
             * @changeColor 点击之后颜色
             * @animate css动画
             * @color 图标颜色
             * @padding 图标间距
             */
            padding: String
        },
        data(){
            return {
                isAddbcColor:'',
                animateClass:''
            };
        },
        computed: {
            computeStyle: function() {
                let _style = '';
                let fontSize = this.size;
                let padding = this.padding;
                let color = this.color;
                let isAddbcColor = this.isAddbcColor;
                if (fontSize) {
                    _style += `font-size: ${fontSize};`;
                }
                if (padding) {
                    _style += `padding: ${padding};`;
                }
                if (color) {
                    _style += `color: ${color};`;
                }
                if (isAddbcColor) {
                    _style += `color: ${isAddbcColor};`;
                }
                return _style;
            }
        }
    };
</script>
<style lang="css">
  @component-namespace mint {
    @component icon {
      position: relative;
      display: inline-block;
    }
  }
    .mt-icon > i{
        font-size: inherit;
    }

    .mint-icon-i{
        display: inline-block;
    }
</style>
