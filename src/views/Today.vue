<template>
    <Layout>
        <h1>今日流水</h1>
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
        top: 40%;
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
        top: 55%;
        left: 50%;
        transform: translateX(-50%);

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
            font-size: 50px;
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


</style>
