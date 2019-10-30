<template>
    <div class="container container-bill">
        <div class="member" @click="jumpMemberBill">成员订单</div>
        <div class="moneyBox">
            <div class="moneyBox-child">
                <div class="moneyBox-money">￥{{month_money}}</div>
                <div class="moneyBox-tip">本月入账</div>
            </div>
            <div class="moneyBox-child">
                <div class="moneyBox-money">￥{{total_money}}</div>
                <div class="moneyBox-tip">累计收益</div>
            </div>
        </div>
        <div class="bill-select-box">
            <div class="bill-select" :class="{'select_active': selectActive == 'selectType'}" id="data-type"
                 @click="selectType">{{typeText}} <img :src="selectActive == 'selectType'? selectImg_active:selectImg"
                                                       alt=""></div>
            <div class="bill-select" :class="{'select_active': selectActive == 'selectSource'}" id="data-source"
                 @click="selectSource">{{sourceText}}<img
                    :src="selectActive == 'selectSource'? selectImg_active:selectImg" alt=""></div>
            <div class="bill-select" :class="{'select_active': selectActive == 'selectMonth'}" id="data-month"
                 @click="selectMonth">{{monthText}}<img :src="selectActive == 'selectMonth'? selectImg_active:selectImg"
                                                        alt=""></div>
        </div>
        <div class="content" v-if="Object.keys(textContent).length == 0">
            <div class="not-content"><img src="../assets/not_content.png" alt=""></div>
            <div>请选择您要查询的账单类型，<br>金额来源和账单月份</div>
        </div>
        <div class="textContent">
            <div class="textContent-line" v-for="item in textContent" v-bind:key="item">
                <div>{{item.money}}元</div>
                <div>{{item.mobile}}</div>
                <div>{{item.date}}</div>
            </div>
            <p v-if="pageType=='finish'" class="finish"> —— 已经到底了 —— </p>
        </div>
    </div>
</template>

<script>
    import https from "../https.js";

    export default {
        name: "bigBill",
        data: function () {
            return {
                token: this.stc,
                pageData: {},
                pageType:'',
                textContent:[],
                month_money:'',
                total_money:'',
                showMsg: false,
                msg: "",
                typeText: '账单类型',
                sourceText: '金额来源',
                monthText: '账单月份',
                selectActive: "",
                selectImg: './images/select.png',
                selectImg_active: './images/select_active.png',

                //请求所需数据
                source:'',
                tid:'',
                date:'',
                pageNum:1,
            };
        },
        created:function(){
            this.getData();
        },
        components: {},
        methods: {
            getData: function () {
                let that = this;
                https.fetchPost("/bill/index.do?sj_h5=1&token=" + that.token)
                    .then(function (res) {
                        if (res.data.code === 1) {
                            that.month_money = res.data.data.user.month_money;
                            that.total_money = res.data.data.user.total_money;
                        } else {
                            // that.msgTip(res.data.message);
                        }
                    })
            },
        }
    }
</script>

<style scoped>
    body {
        font-size: .28rem;
    }

    .container {
        width: 7.5rem;
        margin: 0 auto;
    }
    /* 复制用input框 */
    #oInput {
        position: fixed;
        bottom: 0;
        left: 0;
        opacity: 0;
        z-index: 0;
        width: 1px;
        height: 1px;
    }
    /*提示框*/
    .layalert {
        display: block;
    }

    .layalert .laydetail {
        position: fixed;
        bottom: 20%;
        left: 50%;
        z-index: 999;
        transform: translate(-50%, -50%);

        width: 4rem;
        line-height: 2;
        text-align: center;
        color: #fff;
        background: rgba(0, 0, 0, .6);
        padding: .06rem .2rem;
        border-radius: .1rem;
    }

    .layalert .laydetail p {
        word-break: break-all;
        word-wrap: break-word;
    }
    .container-bill {
        border-top: .15rem solid #F2F3F9;
    }

    .member {
        color: #3F38FE;
        border: 1px solid #3F38FE;
        border-radius: 0.06rem;
        padding: 0.1rem 0.15rem;
        margin: 0.2rem 0.4rem;
        float: right;
        font-size: 0.28rem;
    }

    .moneyBox {
        display: flex;
        margin: 0.9rem 0 0.2rem;
        width: 7.5rem;
        justify-content: space-around;
        color: #fff;
    }

    .moneyBox-child {
        width: 3.39rem;
        height: 1.66rem;
        text-align: center;
    }

    .moneyBox-child:first-child {
        background: url("../assets/month.png");
        background-size: cover;
    }

    .moneyBox-child:last-child {
        background: url("../assets/total.png");
        background-size: cover;
    }

    .moneyBox-money {
        font-size: 0.48rem;
        margin: 0.2rem auto 0.05rem;
    }

    .moneyBox-tip {
        font-size: 0.28rem;
    }

    .bill-select-box {
        display: flex;
        justify-content: space-between;
        text-align: center;
    }

    .bill-select {
        width: 2.34rem;
        height: 0.6rem;
        background-color: #F2F3F9;
        font-size: 0.3rem;
        color: #333333;
        font-weight: bold;
        border: 0.04rem solid #F2F3F9;
    }

    .select_active {
        background-color: #fff;
        color: #1557FF;
    }

    .bill-select img {
        width: 0.16rem;
        height: 0.1rem;
        vertical-align: middle;
    }

    .content {
        font-size: 0.3rem;
        color: #666666;
        text-align: center;
        margin-top: 0.7rem;
    }

    .content img {
        width: 0.85rem;
        height: 0.95rem;

    }
    .textContent{
        padding: 0.1rem;
    }
    .textContent-line{
        display: flex;
        height: 0.88rem;
        align-items: center;
        justify-content: space-between;
        border-bottom: 1px solid #E0E0E0;
        font-size: 0.26rem;
        /*padding: 0 0.8rem;*/
    }

</style>