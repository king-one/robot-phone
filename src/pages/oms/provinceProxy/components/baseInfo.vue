<template>
    <div class="task-detail">
        <div v-if="detail.id">
            <h1  class="mt-20">{{detail.name}}
                <span class="idText">ID: {{detail.id}}</span>
                <ins :class="['medal',{'medal-gold':detail.cityProxyType==3,'medal-sliver':detail.cityProxyType==2,'medal-bronze':detail.cityProxyType==1}]">{{detail.cityProxyType | proxyType}}</ins>
                <div class="fr">
                    <i-switch size="large" v-model="detail.status" :true-value="1" :false-value="0" @on-change="statusChange">
                        <span slot="open">开启</span>
                        <span slot="close">关闭</span>
                    </i-switch>
                </div>
            </h1>
            
    
            <div class="mt-20 ivu-row">
                <div class="ivu-col ivu-col-span-5">
                    <p>代理省份: {{detail.provinceName}}</p>
                </div>
                <div class="ivu-col ivu-col-span-5">
                    <p>AI坐席数: {{detail.robotNum}}</p>
                </div>
                <div class="ivu-col ivu-col-span-5">
                    <p>人工坐席数: {{detail.humanNum}}</p>
                </div>
                <div class="ivu-col ivu-col-span-5">
                    <p>智能坐席数: {{detail.fixedNum}}</p>
                </div>
                 
               
            </div>
            <div class="ivu-row">
                <div class="ivu-col ivu-col-span-5">
                        <p>联系人: {{detail.contactsName||'--'}}</p>
                    </div>
                    <div class="ivu-col ivu-col-span-5">
                        <p>联系号码: {{detail.contactsMobile||'--'}}</p>
                    </div>
                     <div class="ivu-col ivu-col-span-5">
                        <p>创建时间: {{detail.createTime|msToDate}}</p>
                    </div>
            </div>
            <div class="middleboxWrap fleX mt-20">
                 <div class="">
                    <p class="text">
                        <span class="red w115">{{detail.remainPort}}</span> <span class="weq">=</span><span class="fc-blue w115">{{detail.totalPort}}</span><span class="weq">-</span><span class="fc-blue w115">{{detail.usePort}}</span>
                    </p>
                     <p class="text">
                        <span class="w115 mr-40">账户剩余年端口数</span> <span class="w115 mr-40">累计购买年端口数</span><span>累计使用年端口数</span>
                    </p>
                </div>
               <div class="">
                    <p class="text">
                        <span class="red w115">{{detail.monthCardRemainCount}}</span> <span class="weq">=</span><span class="fc-blue w115">{{detail.monthCardAllCount}}</span><span class="weq">-</span><span class="fc-blue w115">{{detail.monthCardUseCount}}</span>
                    </p>
                     <p class="text">
                        <span class="w115 mr-40">账户剩余月端口数</span> <span class="w115 mr-40">累计购买月端口数</span><span>累计使用月端口数</span>
                    </p>
                </div>
               
            </div>
           
        </div>
        <div class="tc pd50" v-else>
            <span>暂无数据</span>
        </div>
    </div>
</template>

<script>
    const proxyMaps = {
        '1': '铜牌代理',
        '2': '银牌代理',
        '3': '金牌代理',
    }
    export default {
        data() {
            return {
                isEditNick: false,
                update: false,
                detail: {
                    account: "",
                    cityName: "",
                    cityProxyType: null,
                    contactsAddress: null,
                    contactsMobile: null,
                    contactsName: null,
                    createTime: null,
                    humanNum: 0,
                    id: null,
                    initPort: 0,
                    mobile: "",
                    name: "",
                    profitAmt: null,
                    remainPort: 0,
                    remarks: null,
                    robotNum: 0,
                    status: 0,
                    superProxy: null,
                    totalAmt: null,
                    totalPort: 0,
                    usePort: 0

                }
            };
        },
        props: {
            id: {
                type: Number,
                default: 0
            },
        },
        watch: {
            id: function(val, oval) {
                this.getDetail();
            }
        },
        filters: {
            proxyType (num){  
                return proxyMaps[`${num}`]
            }
        },
        mounted() {},
        methods: {
            async getDetail() {
                let vm = this;
                let result = await this.$omsApi.provinceProxy.getProvinceProxyDetail(vm.id);
                vm.detail = result;
            },
            statusChange(val) {
                this.$omsApi.provinceProxy.editStatus({
                    id: this.detail.id,
                    status: this.detail.status
                }).then(() => {
                    this.getDetail()
                }).catch(() => {
                    this.getDetail()
                })
            }
        }
    };
</script>


