<template>
<view>
    <message v-if="show" :classType="menssagem.className" :text="menssagem.text"></message>
</view>
</template>

<script>
import NetInfo from "@react-native-community/netinfo";
import message from './MessageComponente';
export default {
    components: {
        'message': message
    },
    mounted() {
        this.verifyConnection();
    },
    data() {
        return {
            menssagem: {
                text: "",
                className: ""
            },
            show: true,
            conection: {}
        }
    },
    methods: {
        verifyConnection() {
            setInterval(() => {
                NetInfo.fetch().then(state => {
                    this.conection = state;
                });
            }, 5000);

        },
        applyMessage() {
            setTimeout(() => {
                this.show = false
            }, 3000);
        }
    },
    watch: {
        conection: function (newValue, oldValue) {
            this.show = true;
            if (newValue.isConnected == false) {
                this.menssagem.text = "Offline - Sem conexão";
                this.menssagem.className = "warn";

            } else {
                this.menssagem.text = "online - Conexão estabelecida";
                this.menssagem.className = "success";
            }
            this.applyMessage();
        }
    },
}
</script>
