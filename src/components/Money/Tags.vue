<template>
    <div class="tags">
        <div class="new">
            <button @click="createTag">新增标签</button>
        </div>
        <div class="ulPress">
            <ul class="current">
                <li v-for="tag in tagList" :key="tag.id"
                    :class="{selected: selectedTags.indexOf(tag)>=0}"
                    @click="toggle(tag)">{{tag.name}}
                </li>
            </ul>
        </div>

    </div>

</template>

<script lang="ts">
    import Vue from 'vue';
    import {Component, Prop} from 'vue-property-decorator';
    import {mixins} from 'vue-class-component';
    import TagHelper from '@/mixins/TagHelper';

    @Component
    export default class Tags extends mixins(TagHelper) {
        selectedTags: string[] = [];

        get tagList() {
            return this.$store.state.tagList;
        }

        created() {
            this.$store.commit('fetchTags');
        }

        toggle(tag: string) {
            const index = this.selectedTags.indexOf(tag);
            if (index >= 0) {
                this.selectedTags.splice(index, 1);
            } else {
                this.selectedTags.push(tag);
            }
            this.$emit('update:value', this.selectedTags);
        }
    }
</script>

<style scoped lang="scss">
    .tags {
        background: white;
        font-size: .7em;
        padding: 3%;
        flex-grow: 1;
        display: flex;
        flex-direction: column-reverse;

        .current {
            height: 8em;
            width: 240%;
            display: flex;
            flex-wrap: wrap;
            overflow: auto;
            &::-webkit-scrollbar {
                display: none;
            }

            > li {
                $bg: #d9d9d9;
                background: #d9d9d9;
                $h: 24px;
                height: $h;
                line-height: $h;
                border-radius: $h/2;
                padding: 0 16px;
                margin-right: 12px;
                margin-top: 4px;
                text-align: center;

                &.selected {
                    background: #ffcc00;
                    color: white;
                }
            }
        }

        > .new {
            padding-top: 16px;

            button {
                background: transparent;
                border: none;
                color: #999;
                border-bottom: 1px solid #ffcc00;
                padding: 0 4px;
            }
        }
    }
    .ulPress{
        overflow: auto;
        &::-webkit-scrollbar {
            display: none;
        }
    }

</style>
