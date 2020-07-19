<template>
  <view style="flex: 1">
    <!-- Component StatusBar -->
    <status-bar />
    <view class="header bg_gray">
      <Image class="logo" :source="require('./assets/logo.png')" />
      <text-input v-model="inputValue" class="box92 input" />

      <touchable-opacity class="box92 bg_green btn_add" :on-press="addItem">
        <text style="color: #fff; text-align: center; font-size: 17">Add new task</text>
      </touchable-opacity>
    </view>

    <scroll-view :content-container-style="{contentContainer: {
        paddingVertical: 20 }}" style="margin-bottom: 5;" >
      <!-- Component todoItem -->
      <todo-item @delete="deleteItem" @done="doneItem" :dados="todo"></todo-item>
    </scroll-view>
 <!-- component Conection -->
<Connection/>
  </view>
</template>

<script>
console.disableYellowBox = true;
import TodoItem from './componetes/TodoItem';
import Connection from './componetes/ConnectionComponent';

export default {
components: {
  'todo-item': TodoItem,
  'Connection': Connection
  },
  data() {
    return {
      inputValue: "",
      check: false,
      todo: [
        { id: 1, name: "Add star to this repository", done: false },
        { id: 2, name: "Fork this project", done: false },
        { id: 3, name: "Install all dependencies", done: false },
        { id: 4, name: "Edit the app", done: false },
        { id: 5, name: "Share with community", done: false },
        { id: 6, name: "activities completed", done: true },
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
    },
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
</style>
