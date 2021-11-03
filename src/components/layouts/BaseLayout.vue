<template>
  <v-container class="main_container pa-0" fluid>
    <v-row no-gutters>
      <v-col cols="3" class="main_left">
        <v-navigation-drawer width="100%" permanent>
          <v-list class="pa-0">
            <v-list-item class="logo_block">
              <v-list-item-content>
                <v-list-item-title class="d-flex align-center">
                  <img class="img" :src="require('../../assets/logo.png')" />
                  <span> Chat </span>
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item v-for="([icon, text], i) in items" :key="i" link>
              <v-list-item-icon>
                <v-avatar color="primary" size="36"> A </v-avatar>
              </v-list-item-icon>

              <v-list-item-content>
                <v-list-item-title>{{ text }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-navigation-drawer>
      </v-col>
      <v-col class="main_right">
        <div class="input_field">
          <v-text-field
            v-model="message"
            solo
            hide-details
            dense
            label="Text"
          ></v-text-field>
          <v-btn depressed @click="send()" color="primary">
            <v-icon> mdi-send </v-icon>
          </v-btn>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "BaseLayout",
  data: () => ({
    items: [
      ["mdi-email", "Inbox"],
      ["mdi-account-supervisor-circle", "Supervisors"],
      ["mdi-clock-start", "Clock-in"],
    ],
    message: "",
  }),
  methods: {
    send() {
      console.log(this.message);
    },
  },
};
</script>

<style>
.main_container {
  display: flex;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.main_left {
  background-color: #eee;
}

.logo_block {
  border-bottom: 1px solid #eee;
}

.logo_block .img {
  height: 50px;
}

.main_right {
  position: relative;
  background-color: lightblue;
}

.input_field {
  display: flex;
  align-items: center;
  padding: 12px;
  gap: 10px;
  position: absolute;
  width: 100%;
  bottom: 0;
}
</style>