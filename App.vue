<template>
<view style="flex: 1" v-if="auth != ''">
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
        paddingVertical: 20 }}" style="margin-bottom: 5;">
        <!-- Component todoItem -->
        <todo-item @delete="deleteItem" @done="doneItem" :dados="todo"></todo-item>
    </scroll-view>
    <!-- component Conection -->
    <Connection />
</view>
<view v-else style="flex: 1; justify-content: center; align-itens: center;" class="bg_gray">
    <text style="color: #fff; text-align: center; font-size: 40; margin-bottom: 30">Login</text>
    <text-input keyboardType="email-address" v-model="email" class="box92 input" />
    <text-input secureTextEntry v-model="password" class="box92 input" />
    <touchable-opacity class="box92 bg_green btn_add" :on-press="login">
        <text style="color: #fff; text-align: center; font-size: 17">Submit</text>
    </touchable-opacity>
</view>
</template>

<script>
console.disableYellowBox = true;
import TodoItem from './componetes/TodoItem';
import Connection from './componetes/ConnectionComponent';
import * as firebase from 'firebase';

// Initialize Firebase
const firebaseConfig = {
    apiKey: "",
    authDomain: "",
    databaseURL: "",
    projectId: ",
    storageBucket: "",
    messagingSenderId: "",
    appId: ""
};

export default {
    components: {
        'todo-item': TodoItem,
        'Connection': Connection
    },
    created() {
        if (firebase.apps == "") {
            firebase.initializeApp(firebaseConfig);
        }
    },
    mounted() {

    },
    data() {
        return {
            inputValue: "",
            check: false,
            auth: "",
            password: "",
            email: "",
            todo: []
        };
    },
    methods: {
        login() {
            firebase.auth().signInWithEmailAndPassword(this.email, this.password).then(result => {
                this.auth = result.user.uid;
                this.loadTasks();
            }).catch(reason => {
                this.auth = "Anonymus"; // Remove this line to authenticate
                console.log(reason);
            });
        },
        loadTasks() {
            firebase.database().ref('/tasks/').child(this.auth).on("value", (snapShot) => {
                this.todo = [];
                for (const iterator in snapShot.val()) {
                    let itm = snapShot.val()[iterator];
                    itm["id"] = iterator;
                    this.todo.push(itm)
                }
            });

        },
        addItem() {
            if (this.inputValue != "") {
                firebase.database().ref('/tasks/').child(this.auth).push({
                    name: this.inputValue,
                    done: false
                })
                this.inputValue = "";
            }
        },
        doneItem(index) {
            firebase.database().ref('/tasks/').child(this.auth).child(this.todo[index].id).set({
                name: this.todo[index].name,
                done: !this.todo[index].done
            });
        },
        deleteItem(index) {
            firebase.database().ref('/tasks/').child(this.auth).child(this.todo[index].id).remove((e)=>{
                console.log(e);
            })
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
</style>
