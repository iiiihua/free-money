<template>
    <div class="wrapper" ref="wrapper">
    </div>
</template>

<script lang="ts">
    import Vue from 'vue';
    import {Component, Prop, Watch} from 'vue-property-decorator';
    import echarts, {EChartOption} from 'echarts';
    import ECharts = echarts.ECharts;

    @Component
    export default class Chart extends Vue {
        @Prop() options?: EChartOption;
        chart?: ECharts;

        mounted() {
            if (this.options === undefined) {
                return console.log('options为空');
            }
            this.chart = echarts.init(this.$refs.wrapper as HTMLDivElement);
            this.chart.setOption(this.options);
        }

        @Watch('options')
        onOptionsChange(newValue: EChartOption) {
            this.chart!.setOption(newValue);
        }

    }
</script>

<style scoped lang="scss">
    .wrapper {
        height: 400px;
    }

</style>
