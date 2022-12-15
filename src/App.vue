<template>
  <v-app id="app">
    <v-main>
      <Users/>
    </v-main>
  </v-app>
</template>

<script>
import Users from "@/components/Users.vue";

export default {
  name: 'App',
  components: {
    Users,
  },
};
</script>
