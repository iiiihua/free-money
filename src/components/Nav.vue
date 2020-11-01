<template>
    <nav class="nav">
        <router-link to="/today" class="item" active-class="selected">
            <Icon name="money"/>
        </router-link>
        <router-link to="/labels" class="item" active-class="selected">
            <Icon name="label"/>
        </router-link>
        <router-link to="/statistics" class="item" active-class="selected">
            <Icon name="statistics"/>
        </router-link>
    </nav>

</template>

<script lang="ts">
    export default {
        name: 'Nav'
    };
</script>

<style scoped lang="scss">
    @import "~@/assets/style/helper.scss";
    nav {
        @extend %outerShadow;
        display: flex;
        flex-direction: row;
        font-size: 12px;
        > .item{
            padding: 1em 0;
            width: 33.33333%;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            .icon{
                width: 32px;
                height: 32px;
            }


        }
        >.item.selected{
            color: white;
            background-color: #ffcc00;
        }

    }

</style>
