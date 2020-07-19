<template>
<view :class="classType">
    <text style="color: #fff; font-size: 16; text-align: center">{{text}}</text>
</view>
</template>

<script>
export default {
    props:{
        text: {type: String},
        classType: {type: String}
    }
}
</script>

<style>
.warn {
    height: 40;
    padding: 10;
    background-color: #ff4444;
}

.success {
    height: 40;
    padding: 10;
    background-color: #00C851;
}
</style>
