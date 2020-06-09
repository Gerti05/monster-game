<template>
  <v-app>
    <v-content>
      <Game />
    </v-content>
  </v-app>
</template>

<script>
import Game from "./components/fullGame";

export default {
  name: "App",
  components: {
    Game,
  },

  data: () => ({
    //
  }),
};
</script>
