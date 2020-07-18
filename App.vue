<template>
  <view style="flex: 1">
    <status-bar />
    <view class="header bg_gray">
      <Image class="logo" :source="require('./assets/logo.png')" />
      <text-input v-model="inputValue" class="box92 input" />

      <touchable-opacity class="box92 bg_green btn_add" :on-press="addItem">
        <text style="color: #fff; text-align: center; font-size: 17">Adicionar Tarefa</text>
      </touchable-opacity>
    </view>

    <scroll-view
      :content-container-style="{contentContainer: {
        paddingVertical: 20 }}"
      style="margin-bottom: 5;"
    >
      <view class="tdItem" v-for="(item, index) in todo" :key="index">
        <check-box
          :on-change="()=>{doneItem(index)}"
          :value="item.done"
          style="width: 10%; padding: 5"
        />
        <text
          style="width: 80%; padding: 5; font-size: 16"
          :style="{textDecorationLine: (item.done ? 'line-through' : 'none'), color: (item.done ? '#ccc' : '#48b884')}"
        >{{item.name}}</text>
        <touchable-opacity
          :on-press="()=>{deleteItem(index)}"
          style="width: 10%; padding: 5; background-color: #f00"
        >
          <text style="color: #fff; text-align: center; align-content: center;">X</text>
        </touchable-opacity>
      </view>
    </scroll-view>
  </view>
</template>

<script>
console.disableYellowBox = true;
export default {
  data() {
    return {
      inputValue: "",
      check: false,
      todo: [
        { id: 1, name: "Atividade 1", done: false },
        { id: 2, name: "Atividade nova", done: false },
        { id: 3, name: "Atividade 3", done: false },
        { id: 4, name: "Atividade 4", done: false },
        { id: 5, name: "Atividade 5", done: false },
        { id: 6, name: "Atividade 6", done: false },
        { id: 7, name: "Atividade 7", done: false },
        { id: 8, name: "Atividade 8", done: false },
        { id: 9, name: "Atividade 9", done: false },
        { id: 10, name: "Atividade 10", done: false },
        { id: 11, name: "Atividade 11", done: false },
        { id: 12, name: "Atividade 12", done: false },
        { id: 13, name: "Atividade 13", done: false },
        { id: 14, name: "Atividade 14", done: false },
        { id: 15, name: "Atividade 15", done: false },
        { id: 16, name: "Atividade 16", done: false },
        { id: 17, name: "Atividade 17", done: false }
      ]
    };
  },
  methods: {
    addItem() {
      if (this.inputValue != "") {
        this.todo.push({ name: this.inputValue, done: false });
        this.inputValue = "";
        this.order();
      }
    },
    doneItem(index) {
      setTimeout(() => {this.order()}, 1000);
      this.todo[index].done = !this.todo[index].done;
    },
    deleteItem(index) {
      this.todo.splice(index, 1);
    },
    order() {
      this.todo.sort((a, b) => {
        if (a.done < b.done) {
          return false;
        } else {
          return true;
        }
      });
    }
  }
};
</script>

<style>
.bg_gray {
  background-color: #4c5e71;
  color: #fff;
}

.bg_green {
  background-color: #48b884;
  color: #fff;
}

.txt_green {
  color: #48b884;
}

.header {
  padding-top: 20;
  padding-bottom: 20;
}

.logo {
  height: 100;
  width: 40%;
  margin-left: 30%;
}
.input {
  background-color: #fff;
  font-size: 18;
  border-radius: 10;
}
.btn_add {
  border-radius: 10;
}

.box92 {
  padding: 10;
  width: 92%;
  margin-left: 4%;
  margin-top: 15;
}

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
