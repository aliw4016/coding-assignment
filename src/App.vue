<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
      class="d-flex justify-content-center"
    >
    <div>
    <h3 > Coding Assignment</h3>
    </div>
    </v-app-bar>

    <v-main>
      <CodingAssignment/>
    </v-main>
  </v-app>
</template>

<script>
import CodingAssignment from './components/CodingAssignment';

export default {
  name: 'App',

  components: {
    CodingAssignment,
  },

  data: () => ({
    //
  }),
};
</script>
<style scoped>
header {
      justify-content: center;
}
</style>