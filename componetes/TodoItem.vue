<template>
  <view>
    <view class="tdItem" v-for="(item, index) in dados" :key="index">
      <check-box :on-change="()=>{$emit('done',index)}"
        :value="item.done" style="width: 10%; padding: 5"/>
      <text style="width: 80%; padding: 5; font-size: 16"
        :style="{textDecorationLine: (item.done ? 'line-through' : 'none'), color: (item.done ? '#ccc' : '#48b884')}">{{item.name}}</text>
      <touchable-opacity :on-press="()=>{$emit('delete', index)}" style="width: 10%; padding: 5; background-color: #f00">
        <text style="color: #fff; text-align: center; align-content: center;">X</text>
      </touchable-opacity>
    </view>
  </view>
</template>
<script>
export default {
  props: {dados: {type: Array}},
  methods: {},
  data() {return {};}
};
</script>

<style>
.tdItem {
  padding: 5;
  width: 96%;
  margin-left: 2%;
  display: flex;
  flex-direction: row;
  margin-top: 5;
  border-bottom-color: #48b884;
  border-bottom-width: 2;
}
</style>