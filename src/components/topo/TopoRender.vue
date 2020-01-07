<template>
<div>
    <!-- 循环样式属性 -->
        <div class="topo-render" :style="layerStyle" v-if="configData.layer">
        <template v-for="(component,index) in configData.components">

            <div class="topo-render-wrapper" :key="index" @click="doClickComponent(component)" @dblclick="doDbClickComponent(component)" :class="{'topo-render-wrapper-clickable': component.action.length > 0 }" v-show="component.style.visible == undefined? true:component.style.visible" :style="{
                        left: component.style.position.x + 'px',
                        top: component.style.position.y + 'px',
                        width: component.style.position.w + 'px',
                        height: component.style.position.h + 'px',
                        backgroundColor: component.style.backColor,
                        zIndex: component.style.zIndex,
                        borderWidth: component.style.borderWidth + 'px',
                        borderStyle: component.style.borderStyle,
                        borderColor: component.style.borderColor,
                        transform: component.style.transform? `rotate(${component.style.transform}deg)`:'rotate(0deg)',
                    }">

                <component v-bind:is="parseView(component)" :detail="component" ref="spirit" />

            </div>

        </template>

    </div>
    <!-- 单击选择数据属性 -->
</div>
</template>

<script>
import qs from "qs";
import axios from "axios";
import ViewText from './control/ViewText';
import ViewImage from './control/ViewImage';

import ViewCircular from './control/canvas/ViewCircular';
import ViewLine from './control/canvas/ViewLine';
import ViewLineArrow from './control/canvas/ViewLineArrow';
import ViewLineWave from './control/canvas/ViewLineWave';
import ViewRect from './control/canvas/ViewRect';
import ViewTriangle from './control/canvas/ViewTriangle';

import ViewChart from './control/chart/ViewChart';
import ViewChartPie from './control/chart/ViewChartPie';
import ViewChartGauge from './control/chart/ViewChartGauge';

import ViewSvgImage from './control/svg/ViewSvgImage';

import topoUtil from './util/topo-util';

export default {
    name: 'TopoRender',
    components: {
        ViewText,
        ViewImage,
        ViewCircular,
        ViewLine,
        ViewLineArrow,
        ViewLineWave,
        ViewRect,
        ViewTriangle,
        ViewChart,
        ViewChartPie,
        ViewChartGauge,
        ViewSvgImage,
    },
    computed: {
        layerStyle: function () {
            var styles = [];
            if (this.configData.layer.backColor) {
                styles.push(`background-color: ${this.configData.layer.backColor}`);
            }
            if (this.configData.layer.backgroundImage) {
                styles.push(`background-image: url("${this.configData.layer.backgroundImage}")`);
            }
            if (this.configData.layer.width > 0) {
                styles.push(`width: ${this.configData.layer.width}px`);
            }
            if (this.configData.layer.height > 0) {
                styles.push(`height: ${this.configData.layer.height}px`);
            }
            var style = styles.join(';');
            return style;
        }
    },
    data() {
        return {
            configData: {

            },
            table: [],
            slaveId: ""
        }
    },
    methods: {
        //获取url中截取路径
        urll() {
            try {
                var a = location.href
                this.slaveId = a.split(/=(?=.)/)[1]
                if (this.slaveId != "") {
                    this.findData()
                    setInterval(()=>{
                        this.findData()
                    },25000)
                } else {
                    this.login();
                }
            } catch {
            }
        },
        findData () {
            axios
                .post("/api/ztsj/six", qs.stringify({
                    slaveId: this.slaveId
                }))
                .then(res => {
                    var Data = res.data.data
                    this.configData = JSON.parse(Data);
                })
        },
        //查看组态页面
        login() {
            axios
                .post("/api/ztsj/select1")
                .then(res => {
                    this.table = res.data.data
                    var temp = res.data.data[0].ztData
                    this.configData = JSON.parse(temp);
                });
        },
        //点击获取id进行查询单条数据
        ShowData(index, row) {
            axios
                .post("/api/ztsj/six", qs.stringify({
                    slaveId: row.slaveId
                }))
                .then(res => {
                    var Data = res.data.data
                    this.configData = JSON.parse(Data);
                });
        },
        parseView(component) {
            return topoUtil.parseViewName(component);
        },
        doClickComponent(component) {
            for (var i = 0; i < component.action.length; i++) {
                var action = component.action[i];

                if (action.type == 'click') {
                    this.handleComponentActuib(action);

                }
            }

        },
        doDbClickComponent(component) {
            for (var i = 0; i < component.action.length; i++) {
                var action = component.action[i];
                if (action.type == 'dblclick') {
                    this.handleComponentActuib(action);

                }
            }
        },
        handleComponentActuib(action) {
            var _this = this;
            if (action.action == 'visible') {
                if (action.showItems.length > 0) {
                    action.showItems.forEach(identifier => {
                        _this.showComponent(identifier, true);
                    });
                }
                if (action.hideItems.length > 0) {
                    action.hideItems.forEach(identifier => {
                        _this.showComponent(identifier, false);
                    });
                }
            } else if (action.action == 'service') {
                _this.sendFun(action);
            }
        },
        showComponent(identifier, visible) {
            var spirits = this.$refs['spirit'];
            for (var i = 0; i < spirits.length; i++) {
                var spirit = spirits[i];
                if (spirit.detail.identifier == identifier) {
                    spirit.detail.style.visible = visible;
                    break;
                }
            }
        },
    },
    mounted() {
        this.urll();
    }

}
</script>

<style lang="scss">
* {
    margin: 0px;
    padding: 0px;
    list-style: none;
}

.yun {
    width: 100px;
    height: 500px;
    float: right;
    margin-top: -500px;
    clear: both;
    overflow: hidden;
}

.yc {
    display: none;
}

.yun ul li {
    position: fixed;
    top: 0px;
    right: 100px;
}

.yun ul li:hover .yc {
    display: inline;
}

.topo-render {
    overflow: auto;
    border: 1px solid blue;
    background-color: white;
    background-clip: padding-box;
    background-origin: padding-box;
    background-repeat: no-repeat;
    background-size: 100% 100%;
    position: relative;
    height: 100%;

    .topo-render-wrapper {
        position: absolute;
    }

    .topo-render-wrapper-clickable {
        cursor: pointer;
    }
}
</style>
