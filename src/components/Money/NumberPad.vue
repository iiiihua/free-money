<template>
    <div class="numberPad">
        <div class="output">
            <div class="gotodayPar">
                <router-link to="/today" class="gotoday">
                    <Icon name="left" class="gotodayIcon"/>
                </router-link>
            </div>
            {{output}}</div>
        <div class="buttons">
            <button @click="inputContent">1</button>
            <button @click="inputContent">2</button>
            <button @click="inputContent">3</button>
            <button @click="remove">delete</button>
            <button @click="inputContent">4</button>
            <button @click="inputContent">5</button>
            <button @click="inputContent">6</button>
            <button @click="clear">clear</button>
            <button @click="inputContent">7</button>
            <button @click="inputContent">8</button>
            <button @click="inputContent">9</button>
            <button @click="ok" class="ok">OK</button>
            <button @click="inputContent" class="zero">0</button>
            <button @click="inputContent">.</button>
        </div>
    </div>
</template>

<script lang="ts">
    import Vue from 'vue';
    import {Component, Prop} from 'vue-property-decorator';
    @Component
    export default class NumberPad extends Vue {
        @Prop(Number) readonly value!: number;
        output = this.value.toString();
        inputContent(event: MouseEvent) {
            const button = (event.target as HTMLButtonElement);
            const input = button.textContent!;
            if (this.output.length === 16) { return; }
            if (this.output === '0') {
                if ('0123456789'.indexOf(input) >= 0) {
                    this.output = input;
                } else {
                    this.output += input;
                }
                return;
            }
            if (this.output.indexOf('.') >= 0 && input === '.') {return;}
            this.output += input;
        }
        remove() {
            if (this.output.length === 1) {
                this.output = '0';
            } else {
                this.output = this.output.slice(0, -1);
            }
        }
        clear() {
            this.output = '0';
        }
        ok() {
            const number = parseFloat(this.output);
            this.$emit('update:value', number);
            this.$emit('submit', number);
            if (this.$store.state.numberPadValue){
                this.output = '0';
                this.$store.state.numberPadValue = null
            }

        }
    }
</script>

<style lang="scss" scoped>
    @import "~@/assets/style/helper.scss";
    .numberPad {
        .output {
            @extend %clearFix;
            @extend %innerShadow;
            font-size: 36px;
            font-family: Consolas, monospace;
            padding: 9px 16px;
            text-align: right;
            height: 72px;
            position: relative;
        }
        .buttons {
            @extend %clearFix;
            > button {
                border: white 1px solid !important;
                width: 25%;
                height: 64px;
                float: left;
                background: #CCCCCC;
                border: none;
                &.ok {
                    height: 64*2px;
                    float: right;
                }
                &.zero {
                    width: 25*2%;
                }
                &:nth-child(1),&:nth-child(2),&:nth-child(3),&:nth-child(4) {
                    border-top: none !important;
                }
                &:nth-child(1),&:nth-child(5),&:nth-child(9),&:nth-child(13) {
                    border-left: none !important;
                }&:nth-child(4),&:nth-child(8),&:nth-child(12) {
                    border-right: none !important;
                }&:nth-child(12),&:nth-child(13),&:nth-child(14) {
                    border-bottom: none !important;
                }

            }
        }
    }
    .gotodayPar{
        position: absolute;
    }
</style>
