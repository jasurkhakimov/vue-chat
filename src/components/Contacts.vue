<template>
  <div>
    <v-list class="pa-0">
      <v-list-item
        :ripple="false"
        color="primary"
        class="logo_block"
        :to="{ name: 'chats' }"
        link
        exact-path
      >
        <v-list-item-content>
          <v-list-item-title class="d-flex align-center">
            <img class="img" :src="require('../assets/logo.png')" />
            <span> Chat - {{ userId }} </span>
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>

    <v-list class="contacts_block pa-0">
      <v-list-item
        :ripple="false"
        color="primary"
        class="contact_link"
        v-for="(item, i) in 1"
        :key="i"
        link
        :to="{ name: 'chat-id', params: { id: userId === 1 ? 2 : 1 } }"
        replace
      >
        <v-list-item-icon>
          <v-avatar color="primary" class="white--text" size="36">
            {{ contact.name[0] }}
          </v-avatar>
        </v-list-item-icon>

        <v-list-item-content>
          <v-list-item-title>{{
            contact.name + " " + (userId === 1 ? 2 : 1)
          }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </div>
</template>

<script>
export default {
  name: "Contacts",
  props: {
    userId: Number,
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
    to: "",
    message: "",
    messages: [],
    error: "lol",
  }),
};
</script>

<style lang='scss'>
</style>