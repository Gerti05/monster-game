<template>
  <v-container>
    <v-card class="mx-auto" max-width="100%">
      <!-- Calls startGame method when Start button is clicked -->
      <div v-if="!game" align="center"> 
        <v-btn @click="startGame()" class="my-3" dark large color="indigo"
          ><font-awesome-icon class="mr-1 fa-lg" icon="gamepad" />Play</v-btn
        >
      </div>
      <!-- Calls firstMove method any time Attack button is clicked -->
      <div v-if="game" class="d-flex justify-space-around">
        <v-btn
          v-bind="size"
          @click="firstMove()"
          class="my-3"
          dark
          color="red lighten-1"
          ><Sword v-bind="size" class="mr-1" />Attack</v-btn
        >
        <v-btn
          v-bind="size"
          @click="special()"
          class="my-3 "
          :disabled="playerSpecialBar < 100"
          :dark="playerSpecialBar >= 100"
          color="orange lighten-1"
          ><Special class="mr-1" />Special</v-btn
        >
        <v-btn
          v-bind="size"
          @click="heal()"
          class="my-3"
          :disabled="playerHealthBar === 100 || playerSpecialBar < 50"
          :dark="playerSpecialBar >= 50 && playerHealthBar != 100"
          color="light-blue darken-1"
          ><Heal class="mr-1" />Healing</v-btn
        >
        <v-btn
          v-bind="size"
          @click="exit()"
          class="my-3"
          dark
          color="grey darken-3 hidden-xs-only"
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
  //Props from fullGame component. game tracks if the game started, playerSpecialBar tracks special bar number, playerHealthBar tracks player health number.
  props: ["game", "playerSpecialBar", "playerHealthBar"],
  methods: {
    //sends event click to fullGame component.
    startGame: function() {
      this.$emit("startGame");
    },
    //Randomizes a number between 1 and 2. If number is 1, the monster gets to attack first, else the player gets to attack first.
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
  computed: {
    //Resizes the buttons based on veiwport size.
    size() {
      const size = { xs: "x-small", sm: "small", lg: "large", xl: "x-large" }[
        this.$vuetify.breakpoint.name
      ];
      return size ? { [size]: true } : {};
    },
  },
};
</script>

<style></style>
