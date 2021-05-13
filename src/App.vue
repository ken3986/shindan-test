<template>
  <v-app>
    <v-main>
      <Shindan/>
    </v-main>
  </v-app>
</template>

<script>
import Shindan from './components/Shindan';

export default {
  name: 'App',

  components: {
    Shindan,
  },

  data: () => ({
    //
  }),
};
</script>

<style>
.v-main {
  margin-top: 2rem;
}
</style>
