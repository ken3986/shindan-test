<template>
  <v-app>
    <v-main>
      <Shindan/>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios'
import Shindan from './components/Shindan';

export default {
  name: 'App',

  components: {
    Shindan,
  },

  data: () => ({
    //
  }),

  created () {
    console.log('App.vue')
    // const res = axios.get('https://shindan-test-develop-l68h9on9v-ken3986.vercel.app/api/test')
    // console.log(res)
  }
};
</script>

<style>
.v-main {
  margin-top: 2rem;
}
</style>
