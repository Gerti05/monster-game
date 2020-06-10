<template>
  <div>
    <Healthbar
      class="inline"
      :player="players[0]"
      :health="playerHealth"
      :special="playerSpecial"
    />
    <Healthbar
      class="inline"
      :player="players[1]"
      :health="monsterHealth"
      :special="monsterSpecial"
    />
    <v-container class="d-flex justify-space-between">
      <Hero class="avatarBackground" />
      <h1 v-if="gameOver && monsterHealth === 0">You Win!</h1>
      <h1 v-else-if="gameOver && playerHealth === 0">You Lose</h1>
      <h1 v-else-if="gameOver && playerHealth === 0 && monsterHealth === 0">
        You Lose
      </h1>
      <Monster class="avatarBackground" />
    </v-container>
    <PlayerActions
      :playerSpecialBar="playerSpecial"
      :game="gameStarted"
      @startGame="start()"
      @exit="exited()"
      @monsterAttack="attacking('playerHealth', 'monsterSpecial')"
      @playerAttack="attacking('monsterHealth', 'playerSpecial')"
      @special="specialAttack('monsterHealth', 'playerSpecial')"
    />
  </div>
</template>

<script>
import Healthbar from "./healthbar";
import PlayerActions from "./playerActions";
import Hero from "../assets/hero";
import Monster from "../assets/monster";

export default {
  data: function() {
    return {
      players: ["You", "Monster"],
      playerHealth: 100,
      monsterHealth: 100,
      playerSpecial: 0,
      monsterSpecial: 0,
      gameStarted: false,
      gameOver: false,
    };
  },
  components: {
    Healthbar,
    PlayerActions,
    Hero,
    Monster,
  },
  methods: {
    exited: function() {
      this.gameStarted = false;
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.playerSpecial = 0;
      this.monsterSpecial = 0;
    },
    start: function() {
      this.gameStarted = true;
      this.gameOver = false;
    },
    attacking: function(e, a) {
      let regularAtt = Math.floor(Math.random() * 5 + 1);
      let randomMonsterSpecial = Math.floor(Math.random() * 2 + 1);

      if (this[e] > regularAtt && !this.gameOver) {
        this[e] -= regularAtt;
      } else if (this[e] < regularAtt && !this.gameOver) {
        this[e] = 0;
      }

      if (this[a] < 100 && !this.gameOver) {
        this[a] += Math.floor(Math.random() * 50 + 1);
      } else if (
        this.monsterSpecial >= 100 &&
        !this.gameover &&
        randomMonsterSpecial === 1 &&
        a === "monsterSpecial"
      ) {
        console.log(this.monsterSpecial);
        this.specialAttack("playerHealth", "monsterSpecial");
      }

      if (this.monsterHealth <= 0 || this.playerHealth <= 0) {
        this.gameOver = true;
      }
    },
    specialAttack: function(e, a) {
      let specialAtt = Math.floor(Math.random() * 5 + 1) + 15;

      if (specialAtt <= this[e] && !this.gameOver) {
        this[e] -= specialAtt;
      } else if (specialAtt > this[e] && !this.gameOver) {
        this[e] = 0;
      }

      if (a === "playerSpecial") {
        this.attacking("playerHealth", "monsterSpecial");
      }

      if (this[e] <= 0) {
        this.gameOver = true;
      }
      if (!this.gameOver) {
        this[a] = 0;
      }
    },
  },
};
</script>

<style>
.inline {
  display: inline-block;
  width: 48%;
  margin: 1%;
}
.avatarBackground {
  background-color: #9575cd;
}
</style>
