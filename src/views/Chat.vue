<template>
  <div>
    <div class="header_block">
      <v-app-bar class="chat_topbar" absolute elevation="0" color="white">
        <v-app-bar-nav-icon>
          <v-avatar color="primary" size="36" class="white--text">
            {{ contact.name[0] }}
          </v-avatar>
        </v-app-bar-nav-icon>

        <v-toolbar-title>{{
          contact.name + ` me - ${userId}`
        }}</v-toolbar-title>

        <v-spacer></v-spacer>

        <v-btn icon>
          <v-icon>mdi-dots-vertical</v-icon>
        </v-btn>
      </v-app-bar>
    </div>

    <!-- MESSAGES BLOCK -->

    <div class="chat_messages">
      <div
        :class="['one_message', msg.to !== userId ? 'to' : '']"
        v-for="(msg, index) in messages"
        :key="index"
      >
        <v-avatar color="primary" size="36" class="white--text">
          {{ contact.name[0] }}
        </v-avatar>
        <div class="text">
          {{ msg.message }}
        </div>
        <div class="accept">
            <v-icon v-if="accepted_messages.has(msg.messageID)" color="#CCFF90" small>
                mdi-check
            </v-icon>
            <v-icon v-else color="" small>
                mdi-timelapse
            </v-icon>
        </div>
        <div class="time">
          {{ getTime(msg.timestamp) }}
        </div>
      </div>
      <div id="bottom"></div>
    </div>

    <div class="input_block">
      <v-textarea
        id="msg"
        auto-grow
        v-model="message"
        solo
        single-line
        hide-details
        class="field white"
        clearable
        clear-icon="mdi-close"
        rows="1"
        label="Message"
        dense
        @keypress.enter="send()"
        @keydown.enter.exact.prevent="send()"
        @keydown.enter.shift.exact.prevent="message += '\n'"
      ></v-textarea>
      <v-btn
        small
        rounded
        elevation="2"
        fab
        @click="send()"
        color="primary"
        :disabled="!message"
      >
        <v-icon size="18"> mdi-send </v-icon>
      </v-btn>
    </div>
  </div>
</template>

<script>
// import { io } from "socket.io-client";
// const socket = io.connect("http://192.168.1.2:7676");
import { nanoid } from "nanoid";

export default {
  name: "Chat",
  props: {
    userId: Number,
    socket: Object,
  },
  components: {},
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
    to: "",
    message: "",
    messages: [],
    error: "lol",
    accepted_messages: new Map(),
  }),

  updated() {
    // This will be called when the component updates
    // try toggling a todo
    this.scrollToEnd();
  },
  created() {
    this.to = this.$route.params.id;
    this.socket.on("message", (args) => {
      // console.log('from server: ', args);
      this.messages.push(args);
    });

    this.socket.on("messageResponse", ({ messageID, accept }) => {
      this.accepted_messages.set(messageID, accept);
      this.$forceUpdate();
    });
  },
  beforeRouteUpdate(to, from, next) {
    if (to.path !== from.path) {
      this.to = to.params.id;
      this.messages = [];
      this.focusMsg();
      next();
    }
  },
  mounted() {
    this.scrollToEnd();
    this.focusMsg();
  },
  methods: {
    send() {
      this.message.trim();
      if (/[^\s]/.test(this.message)) {
        const messageID = nanoid();
        this.message = this.message.replace(/(^\s+[^\s]{0}|\s+[^\s]{0}$)/g, "");
        this.messages.push({
          to: this.to,
          messageID,
          message: this.message,
          timestamp: new Date(),
        });

        // console.log(this.messages);

        this.socket.emit("message", {
          messageID,
          to: Number(this.to),
          message: this.message,
        });
      }
      this.message = "";
    },
    scrollToEnd() {
      this.$el.querySelector("#bottom").scrollIntoView({ behavior: "smooth" });
    },
    focusMsg() {
      this.$el.querySelector("#msg").focus();
    },
    getTime(tmp) {
      const time = new Date(tmp);
      const hours =
        time.getHours() < 10 ? "0" + time.getHours() : time.getHours();
      const minutes =
        time.getMinutes() < 10 ? "0" + time.getMinutes() : time.getMinutes();
      return `${hours}:${minutes}`;
    },
    accepted(msgID) {
        return this.accepted_messages.has(msgID);
    }
  },
};
</script>
