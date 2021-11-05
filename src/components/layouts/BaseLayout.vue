<template>
  <v-container class="main_container pa-0" fluid>
    <v-row no-gutters>
      <v-col cols="3" class="main_left">
        <contacts :userId="userId"></contacts>
      </v-col>
      <v-col class="main_right">
        <router-view :socket="socket" :userId="userId"></router-view>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { io } from "socket.io-client";
import Contacts from "../Contacts.vue";
const socket = io.connect("ws://192.168.1.7:7676", {
  auth: {
    token: 1,
  },
});

export default {
  name: "BaseLayout",
  components: {
    Contacts,
  },
  data: () => ({
    items: [
      {
        id: 1,
        name: "John Snow",
      },
    ],
    contact: {
      id: 1,
      name: "John Snow",
    },
    socket: socket,
    to: "",
    message: "",
    messages: [],
    userId: Math.ceil(Math.random() * 2),
    error: "lol",
  }),
  created: function () {
    this.socket.on("connect", () => {
      this.socket.emit("register", {
        socketId: this.socket.id,
        userId: this.userId,
      });
      console.log(this.userId);
      console.log(this.socket.id);
    });
  },
};
</script>

<style lang='scss'>
</style>