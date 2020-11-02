<template>
    <Layout>
        <Tabs class-prefix="type" :data-source="recordTypeList" :value.sync="type"/>
        <template v-if="ifauto">
            <div class="chart-wrapper" ref="chartWrapper" id="ififauto">
                <Chart class="chart" :options="chartOptions"/>
            </div>
        </template>
        <template v-else>
            <Chart class="chartridos" :options="x"/>
        </template>
        <div style="position: relative">
            <button class="ifauto-button" @click="wantifauto">{{ifbutton}}</button>
        </div>
        <ol v-if="groupedList.length>0" class="olheight">
            <li v-for="(group, index) in groupedList" :key="index">
                <h3 class="title">{{beautify(group.title)}} <span>￥{{group.total}}</span></h3>
                <ol>
                    <li v-for="item in group.items" :key="item.id"
                        class="record"
                    >
                        <span>{{tagString(item.tags)}}</span>
                        <span class="notes">{{item.notes}}</span>
                        <span>￥{{item.amount}} </span>
                    </li>
                </ol>
            </li>
        </ol>
        <div v-else class="noResult">
            目前没有相关记录
        </div>
    </Layout>
</template>
<script lang="ts">
    import Vue from 'vue';
    import {Component, Watch} from 'vue-property-decorator';
    import Tabs from '@/components/Tabs.vue';
    import recordTypeList from '@/constants/recordTypeList';
    import dayjs from 'dayjs';
    import clone from '@/lib/clone';
    import Chart from '@/components/Money/Chart.vue';
    import _ from 'lodash';
    import day from 'dayjs';
    import Button from '@/components/Button.vue';

    @Component({
        components: {Button, Tabs, Chart},
    })
    export default class Statistics extends Vue {
        ifauto = true;
        ifbutton = '显示饼状图';

        tagString(tags: Tag[]) {
            return tags.length === 0 ? '无' : tags.map(t => t.name).join('，');
        }

        mounted() {
            (this.$refs.chartWrapper as HTMLDivElement).scrollLeft = 9999999;
        }

        updated() {
            if (this.ifauto) {
                this.$nextTick(function () {
                    (this.$refs.chartWrapper as HTMLDivElement).scrollLeft = 9999999;
                });
            }
        }

        beautify(string: string) {
            const day = dayjs(string);
            const now = dayjs();
            if (day.isSame(now, 'day')) {
                return '今天';
            } else if (day.isSame(now.subtract(1, 'day'), 'day')) {
                return '昨天';
            } else if (day.isSame(now.subtract(2, 'day'), 'day')) {
                return '前天';
            } else if (day.isSame(now, 'year')) {
                return day.format('M月D日');
            } else {
                return day.format('YYYY年M月D日');
            }
        }

        get recordList() {
            return (this.$store.state as RootState).recordList;
        }

        get keyValueList() {
            const today = new Date();
            const array = [];
            for (let i = 0; i <= 29; i++) {
                const dateString = day(today).subtract(i, 'day').format('YYYY-MM-DD');
                const found = _.find(this.groupedList, {title: dateString});
                array.push({
                    date: dateString, value: found ? found.total : 0

                });
            }
            array.sort((a, b) => {
                if (a.date > b.date) {
                    return 1;
                } else if (a.date === b.date) {
                    return 0;
                } else {
                    return -1;
                }
            });
            return array;
        }

        get x() {
            const array = this.keyValueList;
            return {
                tooltip: {
                    trigger: 'item',
                    formatter: '¥：{c} ({d}%)'
                },
                legend: {
                    left: 10,
                },
                series: [
                    {
                        type: 'pie',
                        radius: ['50%', '70%'],
                        avoidLabelOverlap: false,
                        label: {
                            show: false,
                            position: 'center'
                        },
                        emphasis: {
                            label: {
                                show: true,
                                fontSize: '20',
                                fontWeight: 'bold'
                            }
                        },
                        labelLine: {
                            show: false
                        },
                        data: [
                            {value: array[array.length - 1].value, name: array[array.length - 1].date},
                            {value: array[array.length - 2].value, name: array[array.length - 2].date},
                            {value: array[array.length - 3].value, name: array[array.length - 3].date},
                            {value: array[array.length - 4].value, name: array[array.length - 4].date},
                            {value: array[array.length - 5].value, name: array[array.length - 5].date},
                            {value: array[array.length - 6].value, name: array[array.length - 6].date},
                            {value: array[array.length - 7].value, name: array[array.length - 7].date},

                        ]
                    }
                ]
            };
        }

        wantifauto() {
            this.ifauto = !this.ifauto;
            if (this.ifauto) {
                this.ifbutton = '显示饼状图';
            } else {
                this.ifbutton = '显示折线图';
            }


        }

        get chartOptions() {

            const keys = this.keyValueList.map(item => item.date);
            const values = this.keyValueList.map(item => item.value);
            return {
                grid: {
                    left: 0,
                    right: 0
                },
                xAxis: {
                    type: 'category',
                    data: keys,
                    axisLine: {
                        lineStyle: {
                            color: '#ffcc00'
                        }
                    },
                    axisTick: {
                        alignWithLabel: true
                    },
                    axisLabel: {
                        formatter: function (value: string, index: number) {
                            return value.substr(5);

                        }
                    }
                },
                yAxis: {
                    type: 'value',
                    show: false
                },
                series: [{
                    symbol: 'circle',
                    symbolSize: 12,
                    data: values,
                    type: 'line'
                }],
                tooltip: {
                    show: true,
                    position: 'top',
                    formatter: '¥：' + '{c}'
                }
            };

        }

        get groupedList() {
            const {recordList} = this;
            if (recordList.length === 0) {
                return [];
            }
            const newList = clone(recordList)
                .filter(r => r.type === this.type)
                .sort((a, b) => dayjs(b.createdAt).valueOf() - dayjs(a.createdAt).valueOf());
            if (newList.length === 0) {
                return [] as Result;
            }
            type Result = { title: string; total?: number; items: RecordItem[] }[]
            const result: Result = [{title: dayjs(newList[0].createdAt).format('YYYY-MM-DD'), items: [newList[0]]}];
            for (let i = 1; i < newList.length; i++) {
                const current = newList[i];
                const last = result[result.length - 1];
                if (dayjs(last.title).isSame(dayjs(current.createdAt), 'day')) {
                    last.items.push(current);
                } else {
                    result.push({title: dayjs(current.createdAt).format('YYYY-MM-DD'), items: [current]});
                }
            }
            result.map(group => {
                group.total = group.items.reduce((sum, item) => {
                    return sum + item.amount;
                }, 0);
            });
            return result;
        }

        beforeCreate() {
            this.$store.commit('fetchRecords');
        }

        type = '-';
        recordTypeList = recordTypeList;
    }
</script>

<style scoped lang="scss">
    .echarts {
        max-width: 100%;
    }

    .noResult {
        padding: 16px;
        text-align: center;
    }

    ::v-deep {
        .type-tabs-item {
            background: #C4C4C4;

            &.selected {
                background: white;

                &::after {
                    display: none;
                }
            }
        }

        .interval-tabs-item {
            height: 48px;
        }
    }

    %item {
        padding: 8px 16px;
        line-height: 24px;
        display: flex;
        justify-content: space-between;
        align-content: center;
    }

    .title {
        @extend %item;
    }

    .record {
        background: white;
        @extend %item;
    }

    .notes {
        margin-right: auto;
        margin-left: 16px;
        color: #999;
    }

    .chart {
        width: 430%;

        &-wrapper {
            overflow: auto;

            &::-webkit-scrollbar {
                display: none;
            }
        }
    }

    .ifauto-button {
        border: none;
        background: none;
        color: #c4c4c4;
        position: absolute;
        right: 15px;
        transform: translateY(-30px);

    }

    .olheight {
        height: 1em;
        &::-webkit-scrollbar {
            display: none;
        }
    }
</style>
