<template>
<div class="view-text" :style="{
        fontSize: detail.style.fontSize + 'px',
        fontFamily: detail.style.fontFamily,
        color: detail.style.foreColor,
        textAlign: textAlign,
        lineHeight: lineHeight + 'px',
    }" @click="simpleClick(detail)">
     {{detail.style.text}}
</div>
</template>

<script>
/**
 * by zhangyg 2020-01-08
 * 这里绑定的是输入框的数据
 */
import BaseView from './View';

export default {
    name: 'view-text',
    extends: BaseView,
    props: {

    },
    data() {
        return {
           
        }
    },
    methods: {
         /**
          * 实现对于点击内容的展示
          * "dataBind": {
                    "sn": "",
                    "title": "",
                    "biz": "",
                    "queryParam": {},  这里绑定的是查询条件
                    "dataid": "" 这里是我自己增加的字段用于获取后台的实时数据，保存的是数据点的ID
                },
          * @param detail
          */
         simpleClick(detail) {
              console.log(detail)
         }
    }
}
</script>

<style lang="scss">
.view-text {
    height: 100%;
    width: 100%;
}
</style>
