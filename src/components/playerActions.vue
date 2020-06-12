<template>
  <v-container>
    <v-card class="mx-auto" max-width="100%">
      <div v-if="!game" align="center">
        <v-btn @click="startGame()" class="my-3" dark large color="indigo"
          ><font-awesome-icon class="mr-1 fa-lg" icon="gamepad" />Play</v-btn
        >
      </div>
      <div v-if="game" class="d-flex justify-space-around">
        <v-btn
          @click="firstMove()"
          class="my-3"
          dark
          large
          color="red lighten-1"
          ><Sword class="mr-1" />Attack</v-btn
        >
        <v-btn
          @click="special()"
          class="my-3 "
          :disabled="playerSpecialBar < 100"
          :dark="playerSpecialBar >= 100"
          large
          color="orange lighten-1"
          ><Special class="mr-1" />Special</v-btn
        >
        <v-btn
          @click="heal()"
          class="my-3"
          :disabled="playerHealthBar === 100 || playerSpecialBar < 50"
          :dark="playerSpecialBar >= 50 && playerHealthBar != 100"
          large
          color="light-blue darken-1"
          ><Heal class="mr-1" />Healing</v-btn
        >
        <v-btn @click="exit()" class="my-3" dark large color="grey darken-3"
          ><Quit class="mr-1" />Exit</v-btn
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
  props: ["game", "playerSpecialBar", "playerHealthBar"],
  methods: {
    startGame: function() {
      this.$emit("startGame");
    },
    firstMove: function() {
      let first = Math.floor(Math.random() * 2 + 1);

      if (first === 1) {
        this.monsterAttack();
        this.playerAttack();
      } else {
        this.playerAttack();
        this.monsterAttack();
      }
    },
    exit: function() {
      this.$emit("exit");
    },
    monsterAttack: function() {
      this.$emit("monsterAttack");
    },
    playerAttack: function() {
      this.$emit("playerAttack");
    },
    special: function() {
      this.$emit("special");
    },
    heal: function() {
      this.$emit("heal");
    },
  },
  components: {
    Sword,
    Special,
    Heal,
    Quit,
  },
};
</script>

<style></style>
