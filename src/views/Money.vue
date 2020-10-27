<template>
    <Layout class-prefix="layout">
        <NumberPad :value.sync="record.amount" @submit="saveRecord"/>
        <Tabs :data-source="recordTypeList"
              :value.sync="record.type"/>

        <div class="notes">
            <FormItem field-name="备注"
                      placeholder="在这里输入备注"
                      :value="record.notes"
                      @update:value="onUpdateNotes"
            />
        </div>
        <div class="createdAt">
            <FormItem field-name="日期"
                      type="date"
                      placeholder="在这里输入日期"
                      :value="record.createdAt"
                      @update:value="onUpdateDate"
            />
        </div>
        <Tags @update:value="record.tags = $event"/>
        <div class="gotodayPar">
            <router-link to="/today" class="gotoday">
                <Icon name="left" class="gotodayIcon"/>
            </router-link>
            <span>记一笔</span>
        </div>

    </Layout>
</template>

<script lang="ts">
    import Vue from 'vue';
    import NumberPad from '@/components/Money/NumberPad.vue';
    import FormItem from '@/components/Money/FormItem.vue';
    import Tags from '@/components/Money/Tags.vue';
    import {Component} from 'vue-property-decorator';
    import Tabs from '@/components/Tabs.vue';
    import recordTypeList from '@/constants/recordTypeList';

    @Component({
        components: {Tabs, Tags, FormItem, NumberPad},
    })
    export default class Money extends Vue {
        get recordList() {
            return this.$store.state.recordList;
        }

        recordTypeList = recordTypeList;
        record: RecordItem = {
            tags: [], notes: '', type: '-', amount: 0, createdAt: new Date().toISOString()
        };

        created() {
            this.$store.commit('fetchRecords');
        }

        onUpdateNotes(value: string) {
            this.record.notes = value;
        }

        onUpdateDate(value: string) {
            this.record.createdAt = value;
        }

        saveRecord() {
            if (!this.record.tags || this.record.tags.length === 0) {
                return window.alert('请至少选择一个标签');
            }
            if (this.record.amount <= 0) {
                return window.alert('请输入金额')
            }
            this.$store.commit('createRecord', this.record);
            if (this.$store.state.createRecordError) {
                this.$store.state.numberPadValue = 'ok'
                window.alert('已保存');
            }
            this.record.notes = '';
        }
    }
</script>

<style lang="scss" scoped>
    ::v-deep .layout-content {
        display: flex;
        flex-direction: column-reverse;
    }

    .notes {
        padding: 12px 0;
    }
    .gotodayPar{
        background: white;
        span{
            position: absolute;
            margin-top: 10px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 22px;
        }
    }
    .gotoday{
        width: 50px;
        min-height: 30px;
        background: white;
        &Icon {
            padding-left: 5px;
            font-size: 35px;
        }
    }
</style>
