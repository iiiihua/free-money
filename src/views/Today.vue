<template>
    <Layout>
        <h1>今日流水</h1>
        <div class="main-container">
            <div class="window">
                <div class="moon"></div>
                <div class="star st1"></div>
                <div class="star st2"></div>
                <div class="star st3"></div>
                <div class="star st4"></div>
                <div class="star st5"></div>
                <div class="star st6"></div>
                <div class="central-dash"></div>
                <div class="bottom-dash"></div>
                <div class="body"></div>
                <div class="butt"></div>
                <div class="ear-left"></div>
                <div class="ear-right"></div>
                <div class="tail"></div>
                <div class="mt1"></div>
                <div class="mount mt2"></div>
                <div class="mount mt3"></div>
                <div class="mount mt4"></div>
                <div class="mt5"></div>
                <div class="mount mt6"></div>
            </div>
        </div>
        <div class="todayOut"><span>今日支出</span>
            <div>{{getmoney}}</div>
        </div>
        <div class="todayIn"><span>今日收入</span>
            <div>{{setmoney}}</div>
        </div>
        <div class="readNote">
            <router-link to="/today/money">记一笔哈</router-link>
        </div>
    </Layout>
</template>

<script lang="ts">
    import Vue from 'vue';
    import {Component} from 'vue-property-decorator';
    import day from 'dayjs';
    import _ from 'lodash';

    @Component
    export default class Today extends Vue {
        getmoney = 0;
        setmoney = 0;

        beforeMount() {
            let x = 0;
            let y = 0;
            const xy = 0;
            this.$store.commit('fetchRecords');
            const oldList = this.$store.state.recordList;
            const thisdata = day(new Date()).format('YYYY-MM-DD');
            for (let i = 0; i < oldList.length; i++) {
                if (day(oldList[i].createdAt).format('YYYY-MM-DD') === thisdata) {
                    if (oldList[i].type === '-') {
                        x += oldList[i].amount;
                    } else {
                        y += oldList[i].amount;
                    }
                }
            }
            this.getmoney = x;
            this.setmoney = y;
        }
    }
</script>

<style lang="scss" scoped>
    h1 {
        margin-top: 20px;
        text-align: center;

    }

    .todayOut {
        position: absolute;
        top: 28%;
        left: 50%;
        transform: translateX(-50%);

        > span {
            min-width: 100px;
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            font-size: 20px;
            color: #00ffff;
            text-align: center;
        }

        > div {
            padding: 10px;
            font-size: 80px;
            color: #99ccff;
        }

    }

    .todayIn {
        position: absolute;
        top: 45%;
        left: 50%;

        > span {
            min-width: 100px;
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            color: #cc3399;
            text-align: center;
        }

        > div {
            padding: 10px;
            font-size: 40px;
            color: #9900ff;
        }


    }

    .readNote {
        position: absolute;
        bottom: 150px;
        left: 50%;
        transform: translateX(-50%);
        color: #ffcc00;
        font-size: 20px;
        background: transparent;

    }

    html {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }

    *, *:after, *:before {
        box-sizing: inherit;
    }

    body {
        background: #C9533E;
    }

    .window {
        background: #29467A;
        outline: 12px solid #2D2623;
        border: solid #439ABF;
        border-width: 0px 5px;
        position: relative;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        margin: 6em auto 1em;
        width: 260px;
        height: 390px;
    }

    .moon-container {
        position: relative;
        top: 2em;
        left: 2em;
    }

    .moon {
        width: 80px;
        height: 80px;
        background: transparent;
        border-radius: 50px;
        box-shadow: -15px 8px 0 1px #e8ae4a;
        position: absolute;
        left: 3em;
        top: 2em;
    }

    .star {
        width: 7px;
        height: 7px;
        border-radius: 50px;
        background: #E8AE4A;
        position: absolute;
    }

    @keyframes twinkle {
        50% {
            box-shadow: 0 0 15px 3px rgba(232, 174, 74, 0.5);
        }
    }

    .st1 {
        bottom: 230px;
        left: 30px;
        animation: twinkle 2s infinite;
        animation-delay: 11s;
    }

    .st2 {
        bottom: 145px;
        left: 150px;
        animation: twinkle 2s infinite;
        animation-delay: 5s;
    }

    .st3 {
        bottom: 120px;
        left: 40px;
        animation: twinkle 2s infinite;
        animation-delay: 6s;
    }

    .st4 {
        bottom: 350px;
        left: 220px;
        animation: twinkle 2s infinite;
        animation-delay: 7s;
    }

    .st5 {
        bottom: 340px;
        left: 120px;
        animation: twinkle 2s infinite;
        animation-delay: 8s;
    }

    .st6 {
        bottom: 235px;
        left: 200px;
        animation: twinkle 2s infinite;
        animation-delay: 2s;
    }

    .central-dash {
        width: 280px;
        height: 12px;
        background: #2D2623;
        position: absolute;
        bottom: 0;
        top: 0;
        margin: auto -1em;
    }

    .central-dash::before {
        content: ' ';
        width: 250px;
        height: 5px;
        background: #439ABF;
        position: absolute;
        bottom: 100%;
        left: 1em;
    }

    .bottom-dash {
        width: 316px;
        height: 12px;
        background: #2D2623;
        position: absolute;
        top: 100%;
        margin: 0 -2em;
        z-index: 1;
    }

    .bottom-dash::before, .bottom-dash::after {
        content: ' ';
        width: 316px;
        height: 4px;
        background: #29467A;
        position: absolute;
        bottom: 100%;
        z-index: 1;
    }

    .bottom-dash::after {
        background: #439ABF;
        width: 285px;
        left: 15px;
    }

    .body {
        height: 125px;
        width: 50px;
        border-radius: 40px 40px 0 0;
        background: #2D2623;
        position: absolute;
        bottom: 0;
        left: 35%;
        z-index: 1;
    }

    .butt {
        background: #2D2623;
        height: 80px;
        width: 40px;
        border-radius: 0 80px 80px 0;
        position: absolute;
        bottom: -3%;
        left: 53%;
        z-index: 1;
    }

    .ear-left {
        height: 0;
        width: 0;
        border-bottom: 40px solid #2d2623;
        border-right: 33px solid transparent;
        position: absolute;
        bottom: 100px;
        left: 88px;
    }

    .ear-right {
        height: 0;
        width: 0;
        border-bottom: 40px solid #2d2623;
        border-left: 33px solid transparent;
        position: absolute;
        bottom: 100px;
        left: 105px;
    }

    .tail {
        background-color: #2D2623;
        height: 80px;
        width: 10px;
        border-radius: 5px;
        position: absolute;
        top: 100%;
        left: 58%;
        transform-origin: top;
        transform: rotate(-15deg);
        animation: tail 4s infinite;
    }

    @keyframes tail {
        50% {
            transform: rotate(10deg);
        }
    }

    .mount {
        height: 0;
        width: 0;
        border-bottom: 60px solid #22366D;
        border-right: 20px solid transparent;
        border-left: 20px solid transparent;
        position: absolute;
        bottom: 0;
    }

    .mt1 {
        height: 0;
        width: 0;
        border-bottom: 60px solid #22366D;
        border-right: 20px solid transparent;
        position: absolute;
        bottom: 0;
        left: 0;
    }

    .mt2 {
        left: 2.5%;
        bottom: -.7em;
        z-index: 0;
    }

    .mt3 {
        left: 35px;
    }

    .mt4 {
        left: 78%;
    }

    .mt5 {
        height: 0;
        width: 0;
        border-bottom: 60px solid #22366D;
        border-left: 20px solid transparent;
        position: absolute;
        bottom: 0;
        right: 0;
    }

    .mt6 {
        left: 65%;
        bottom: -.7em;
    }


</style>
