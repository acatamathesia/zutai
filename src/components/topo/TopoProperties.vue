<template>
<div class="topo-properties">
    <div class="topo-properties-nav">
        <!-- <q-select v-model="curComponent" :options="componentOptions" @input="changeComponent" style="margin-right:0px;height:43px;border:none;" /> -->
        <template v-if="isLayer">
            <q-input v-model="topoData.name" />
        </template>
        <template v-if="configObject != null && isLayer == false">
            <q-input v-model="configObject.name" />
        </template>
    </div>
    <template v-if="configObject != null && isLayer == false">
        <div class="topo-properties-tabs">
            <div class="topo-properties-tab" @click="changeTab(0)" :class="{'topo-properties-tab-active': tabIndex == 0}">样式</div>
            <div class="topo-properties-tab" @click="changeTab(1)" :class="{'topo-properties-tab-active': tabIndex == 1}">数据</div>
            <div class="topo-properties-tab" @click="changeTab(2)" :class="{'topo-properties-tab-active': tabIndex == 2}">行为</div>
        </div>
        <div class="topo-properties-table">
            <div v-show="tabIndex == 0">
                <table style="display: none">
                    <tr>
                        <td width="50%" style="padding:5px 0px;background-color:#eee;">属性</td>
                        <td width="50%" style="padding:5px 0px;background-color:#eee;">值</td>
                    </tr>
                </table>
                <q-list separator no-border>
                    <q-expansion-item label="Position" default-opened>
                        <table>
                            <tr>
                                <td width="50%">X</td>
                                <td width="50%">
                                    <q-input type="number" suffix="px" v-model.lazy="configObject.style.position.x" style="padding-right:5px;" />
                                </td>
                            </tr>
                            <tr>
                                <td>Y</td>
                                <td>
                                    <q-input type="number" suffix="px" v-model.lazy="configObject.style.position.y" style="padding-right:5px;" />
                                </td>
                            </tr>
                            <tr>
                                <td>W</td>
                                <td>
                                    <q-input type="number" suffix="px" v-model.lazy="configObject.style.position.w" style="padding-right:5px;" />
                                </td>
                            </tr>
                            <tr>
                                <td>H</td>
                                <td>
                                    <q-input type="number" suffix="px" v-model.lazy="configObject.style.position.h" style="padding-right:5px;" />
                                </td>
                            </tr>
                        </table>
                    </q-expansion-item>

                    <q-expansion-item label="Border" default-opened>
                        <table>
                            <tr>
                                <td width="50%">边框</td>
                                <td width="50%">
                                    <q-input  style="padding-right:5px;" v-model.lazy="configObject.style.borderWidth" />
                                </td>
                            </tr>
                            <tr>
                                <td>BorderStyle</td>
                                <td>
                                    <q-select v-model="configObject.style.borderStyle" :options="borderStyleOptions" />
                                </td>
                            </tr>
                            <tr>
                                <td>BorderColor</td>
                                <td>
                                    <q-input filled v-model="configObject.style.borderColor" class="my-input">
                                        <template v-slot:append>
                                            <q-icon name="colorize" class="cursor-pointer">
                                                <q-popup-proxy transition-show="scale" transition-hide="scale">
                                                    <q-color v-model="configObject.style.borderColor" />
                                                </q-popup-proxy>
                                            </q-icon>
                                        </template>
                                    </q-input>
                                </td>
                            </tr>
                        </table>
                    </q-expansion-item>

                    <q-expansion-item label="Base" default-opened>
                        <table>
                            <tr>
                                <td width="50%">Visible</td>
                                <td width="50%">
                                    <q-select v-model="configObject.style.visible" :options="[{label: 'Visible',value: true},{label: 'Invisible',value: false}]" />
                                </td>
                            </tr>
                            <tr>
                                <td>z-index</td>
                                <td>
                                    <q-input type="number" v-model.lazy="configObject.style.zIndex" />
                                </td>
                            </tr>
                            <tr>
                                <td>Rolate</td>
                                <td>
                                    <q-input type="number" v-model.lazy="configObject.style.transform" suffix="deg" style="padding-right:5px;" />
                                </td>
                            </tr>
                            <tr>
                                <td>BackColor</td>
                                <td>
                                    <q-input filled v-model="configObject.style.backColor" class="my-input">
                                        <template v-slot:append>
                                            <q-icon name="colorize" class="cursor-pointer">
                                                <q-popup-proxy transition-show="scale" transition-hide="scale">
                                                    <q-color v-model="configObject.style.backColor" />
                                                </q-popup-proxy>
                                            </q-icon>
                                        </template>
                                    </q-input>
                                </td>
                            </tr>
                            <tr v-if="configObject.style.url != undefined && configObject.style.url != null">
                                <td>URL</td>
                                <td>
                                    <q-input v-model.lazy="configObject.style.url" />
                                </td>
                            </tr>
                            <tr v-if="configObject.style.text != undefined">
                                <td>Text</td>
                                <td>
                                    <q-input v-model.lazy="configObject.style.text" />
                                    <!-- <input v-model.lazy="configObject.style.text" /> -->
                                </td>
                            </tr>
                        
                            <tr v-if="configObject.style.textAlign != undefined">
                                <td>uid</td>
                                <td>
                                    <q-select v-model="configObject.style.textAlign" :options="textAlignOptions" />
                                </td>
                            </tr>
                            <tr>
                                <td>ForeColor</td>
                                <td>
                                    <q-input filled v-model="configObject.style.foreColor" class="my-input">
                                        <template v-slot:append>
                                            <q-icon name="colorize" class="cursor-pointer">
                                                <q-popup-proxy transition-show="scale" transition-hide="scale">
                                                    <q-color v-model="configObject.style.foreColor" />
                                                </q-popup-proxy>
                                            </q-icon>
                                        </template>
                                    </q-input>
                                </td>
                            </tr>
                            <tr v-if="configObject.style.fontFamily != undefined">
                                <td>Font Family</td>
                                <td>
                                    <q-select v-model="configObject.style.fontFamily" :options="fontFamilyOptions" />
                                </td>
                            </tr>
                            <tr v-if="configObject.style.fontSize != undefined">
                                <td>Font Size</td>
                                <td>
                                    <q-input type="number" suffix="px" style="padding-right:5px;" v-model.lazy="configObject.style.fontSize" />
                                </td>
                            </tr>
                            <tr v-if="configObject.style.radius != undefined">
                                <td>Radius</td>
                                <td>
                                    <q-input type="number" v-model.lazy="configObject.style.radius" />
                                </td>
                            </tr>
                            <tr v-if="configObject.style.lineWidth != undefined">
                                <td>LineWidth</td>
                                <td>
                                    <q-input type="number" suffix="px" style="padding-right:5px;" v-model.lazy="configObject.style.lineWidth" />
                                </td>
                            </tr>
                        </table>
                    </q-expansion-item>
                </q-list>
            </div>
            <div v-show="tabIndex == 1">
                <div id="model-temp" style="width:100%;height:100%;text-align:center;">
                    <i class="el-icon-setting text-icon" style="font-size: 24px;line-height:55px;">数据配置</i>
                    <el-form ref="form" :model="form" label-width="80px">
                        <el-form-item label="设备分组">
                            <el-select v-model="form.group" placeholder="请选择设备分组">
                                <el-option v-for="(item,index) in groups" :key="index" :label="item.groupName" :value="item.id" :title="item.groupDescrib"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="设备">
                            <el-select  v-model="form.facility" placeholder="请选择设备">
                                <el-option v-for="(item,index) in facilitys" :key="index" :label="item.sebName" :value="item.id" :title="'地址-'+item.sebAddress"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="从机">
                            <el-select v-model="form.slave" placeholder="请选择从机">
                                <el-option v-for="(item,index) in slaves" :key="index" :label="item.slaveName" :value="item.templateId"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="数据点">
                            <el-select v-model="form.node" placeholder="请选择数据点">
                                <el-option v-for="(item,index) in nodes" :key="index" :label="item.datapointName" :value="item.id"></el-option>
                            </el-select>
                        </el-form-item>
                    </el-form>
                    <el-button @click="save">保存数据信息</el-button>
                    <el-divider>
                        <i class="el-icon-mobile-phone">数据点信息</i>
                    </el-divider>
                </div>
            </div>
            <div v-show="tabIndex == 2">
                自己恢复
            </div>
        </div>
    </template>
    <template v-if="isLayer">
        <table style="margin-top: 10px">
            <tr>
                <td width="40%">BackColor</td>
                <td>
                    <q-input v-model.lazy="topoData.layer.backColor" />
                </td>
            </tr>
            <tr>
                <td>BackImage</td>
                <td>
                    <q-input v-model.lazy="topoData.layer.backgroundImage" />
                </td>
            </tr>
<!--            <tr>-->
<!--                <td>分辨率</td>-->
<!--                <td>-->
<!--                    <q-select v-model="layerWH" :options="whOptions" />-->
<!--                </td>-->
<!--            </tr>-->
            <tr v-if="layerWH == 'custom'">
                <td>W</td>
                <td>
                    <q-input v-model.lazy="topoData.layer.width" />
                </td>
            </tr>
            <tr v-if="layerWH == 'custom'">
                <td>H</td>
                <td>
                    <q-input v-model.lazy="topoData.layer.height" />
                </td>
            </tr>
        </table>
    </template>

</div>
</template>

<script>
import {
    mapActions,
    mapGetters,
    mapState,
    mapMutations
} from "vuex";
import {
    Form,
    FormItem,
    Select,
    Option,
    Button,
    Divider,
    Message
} from "element-ui";
import qs from "qs";
import axios from "axios";
export default {
    name: "TopoProperties",
    data() {
        return {
            data: [],
            tabIndex: 0,
            fontFamilyOptions: [
                "Arial",
                "Helvetica",
                "sans-serif",
                "宋体",
                "黑体",
                "微软雅黑"
            ],
            textAlignOptions:[],
            borderStyleOptions: ["solid", "dashed", "dotted"],
            whOptions: [
                "1024x768",
                "1366x768",
                "1280x800",
                "1440x900",
                "1600x900",
                "1920x1080",
                "custom"
            ],
            layerWHTemp: "",
            form: {
                group: "",
                facility: "",
                slave: "",
                temp: "",
                node: ""
            },
            groups: [],
            facilitys: [],
            slaves: [],
            nodes: [],
            id: ''
        };
    },
    components: {
        "el-form": Form,
        "el-form-item": FormItem,
        "el-select": Select,
        "el-option": Option,
        "el-button": Button,
        "el-divider": Divider
    },
    computed: {
        layerWH: {
            get: function () {
                if (!this.topoData.layer.width || !this.topoData.layer.height) {
                    this.topoData.layer.width = 1600;
                    this.topoData.layer.height = 900;
                }
                if (this.layerWHTemp == "") {
                    var wh = this.topoData.layer.width + "x" + this.topoData.layer.height;
                    if (this.whOptions.indexOf(wh, 0) == -1) {
                        this.layerWHTemp = "custom";
                    } else {
                        this.layerWHTemp = wh;
                    }
                } else {
                    var wh = this.topoData.layer.width + "x" + this.topoData.layer.height;
                    if (this.whOptions.indexOf(wh, 0) == -1) {
                        this.layerWHTemp = "custom";
                    }
                }
                return this.layerWHTemp;
            },
            set: function (val) {
                this.layerWHTemp = val;
                if (val == "custom") {} else {
                    var wh = val.split("x");
                    this.topoData.layer.width = parseInt(wh[0]);
                    this.topoData.layer.height = parseInt(wh[1]);
                }
            }
        },
        ...mapState({
            topoData: state => state.topoEditor.topoData,
            selectedComponents: state => state.topoEditor.selectedComponents,
            selectedComponentMap: state => state.topoEditor.selectedComponentMap,
            isLayer: state => state.topoEditor.selectedIsLayer,
            configObject: state => state.topoEditor.selectedComponent
        })
    },
    methods: {
        initPage(configData) {
            this.configData = configData;
        },
        changeTab(tabIndex) {
            this.tabIndex = tabIndex;
        },
        bindData(configObject, index, isLayer) {
            this.configObject = configObject;
            this.isLayer = isLayer;
            if (isLayer === false) {}
        },
        generateTargetComponentOptions() {
            var options = [];
            this.topoData.components.forEach(component => {
                if (this.configObject.identifier != component.identifier) {
                    options.push({
                        label: component.name || component.type,
                        value: component.identifier
                    });
                }
            });
            return options;
        },
        removeAction(index) {
            this.configObject.action.splice(index, 1);
        },
        addAction() {
            var action = {
                type: "click",
                action: "visible",
                showItems: [],
                hideItems: []
            };
            this.configObject.action.push(action);
        },
        selGroupInformation() {
            axios.post("/api/iotgroup/list", {}).then(res => {
                this.groups = res.data.data;
            });
        },
        selAllInformation(url, id) {
            return axios.post("/api/" + url, qs.stringify(id)).then(res => res.data);
        },
        //保存数据节点id
        save() {
            axios
                .post("/api/node/save", {
                    id: this.form.node,
                })
                .then(res => {
                    //获取data
                    this.data = res.data
                    this.id = this.data.data.id
                    this.select() // 调用select 绑定当前数据点的基本信息
                    this.suiji()
                });

        },
        //绑定当前
        select() {
            // 再次调用接口查询数据点绑定信息
            axios
                .post("/api/node/currentEsvalue", qs.stringify({
                    id: this.id
                }))
                .then(res => {
                    this.data = res.data
                    this.configObject.style.text = this.data.currentValue
                    this.configObject.name=this.data.datapointName
                    this.configObject.dataBind.dataId = this.id // 这里绑定后台查询时需要的id信息
                    Message.success("数据点绑定成功！");
                });
        },
        //生成随机uid
        suiji() {
            var s = [];
            var hexDigits = "0123456789abcdef";
            for (var i = 0; i < 36; i++) {
                s[i] = hexDigits.substr(Math.floor(Math.random() * 0x10), 1);
            }
            s[14] = "4"; 
            s[19] = hexDigits.substr((s[19] & 0x3) | 0x8, 1); 
            var uuid = s.join("");
            this.text = uuid
            this.textAlignOptions.push(this.text)
        }
    },

    mounted() {
        this.selGroupInformation()
        this.suiji()
    },
    watch: {
        "form.group": {
            handler(nVal, oVal) {
                this.selAllInformation("/iotgroup/select1", {
                    id: nVal
                }).then(res => {
                    this.facilitys = res.data[0].seb;
                    this.form.facility = "";
                    this.form.slave = "";
                    this.form.node = "";
                    this.slaves = [];
                    this.nodes = [];
                });
            }
        },
        "form.facility": {
            handler(nVal, oVal) {
                if (nVal === "") return;
                this.selAllInformation("/iotslave/slave", {
                    sebId: nVal
                }).then(res => {
                    this.slaves = res.data;
                    this.form.slave = "";
                    this.form.node = "";
                    this.nodes = [];
                });
            }
        },
        "form.slave": {
            handler(nVal, oVal) {
                if (nVal === "") return;
                this.selAllInformation("/point/querylist", {
                    id: nVal
                }).then(res => {
                    this.nodes = res.data;
                    this.form.node = "";
                });
            }
        },
        "configObject.name": {
            handler(oVal, nVal) {
                this.form.facility = "";
                this.form.slave = "";
                this.form.node = "";
                this.slaves = [];
                this.nodes = [];
            }
        }
    }
};
</script>

<style lang="scss">
.topo-properties {
    border: #ccc solid 1px;
    background-color: white;
    height: 100%;

    .topo-properties-tabs {
        height: 35px;
        display: flex;
        border-bottom: #ccc solid 1px;
        background-color: white;

        .topo-properties-tab {
            height: 35px;
            text-align: center;
            line-height: 35px;
            flex: 1;
            color: #666;
        }

        .topo-properties-tab+.topo-properties-tab {
            border-left: #ccc solid 1px;
        }

        .topo-properties-tab:hover {
            cursor: pointer;
        }

        .topo-properties-tab-active {
            color: #000;
            border-bottom: #3388ff solid 2px;
            font-weight: bold;
        }
    }

    .topo-properties-table {
        overflow-x: hidden;
        overflow-y: auto;
        height: calc(100% - 92px);

        table {
            width: 100%;
            border-collapse: collapse;

            td {
                text-align: center;
                padding: 0px;
                border: #ccc solid 1px;
            }

            .q-input {
                border: none;
            }

            .q-select {
                border: none;
                margin-right: 0px;
            }
        }
    }

    .not-surpport {
        margin: 20px auto;
        text-align: center;
    }
}
</style>
