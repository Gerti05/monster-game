<template>
  <v-container>
    <v-card class="mx-auto" max-width="100%">
      <div v-if="!gameStarted" align="center">
        <v-btn @click="gameStarted = true" class="my-3" dark large color="indigo"
          ><font-awesome-icon class="mr-1 fa-lg" icon="gamepad" />Play</v-btn
        >
      </div>
      <div v-if="gameStarted" align="center">
        <v-btn @click="gameStarted = true" class="my-3 mr-12" dark large color="red lighten-1"
          ><Sword class="mr-1"/>Attack</v-btn
        >
        <v-btn @click="gameStarted = true" class="my-3 mr-12" dark large color="orange lighten-1"
          ><Special class="mr-1"/>Special</v-btn
        >
        <v-btn @click="gameStarted = true" class="my-3 mr-12" dark large color="light-blue darken-1"
          ><Heal class="mr-1"/>Healing</v-btn
        >
        <v-btn @click="gameStarted = true" class="my-3" dark large color="grey darken-3"
          ><Quit class="mr-1"/>Exit</v-btn
        >
      </div>
    </v-card>
  </v-container>
</template>

<script>
import Sword from "../assets/sword";
import Special from "../assets/special";
import Heal from "../assets/healing";
import Quit from "../assets/quit";

export default {
  data: function() {
    return {
      gameStarted: false
    };
  },
  components: {
    Sword,
    Special,
    Heal,
    Quit
  }
};
</script>

<style></style>
