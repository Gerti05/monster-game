<template>
  <div>
    <Healthbar
      class="inline"
      :player="players[0]"
      :health="playerHealth"
      :special="playerSpecial"
      :reverseBar="!progressReverse"
    />
    <Healthbar
      class="inline barDirection"
      :player="players[1]"
      :health="monsterHealth"
      :special="monsterSpecial"
      :reverseBar="progressReverse"
    />
    <v-container class="d-flex justify-space-between">
      <Hero class="avatarBackground" />
      <h1 v-if="gameOver && monsterHealth === 0 && playerHealth > 0">
        You Win!
      </h1>
      <h1 v-else-if="gameOver && playerHealth === 0 && monsterHealth > 0">
        You Lose!
      </h1>
      <h1 v-else-if="gameOver && playerHealth === 0 && monsterHealth === 0">
        Draw!
      </h1>
      <Monster class="avatarBackground" />
    </v-container>
    <PlayerActions
      :playerSpecialBar="playerSpecial"
      :playerHealthBar="playerHealth"
      :game="gameStarted"
      @startGame="start()"
      @exit="exited()"
      @monsterAttack="attacking('playerHealth', 'monsterSpecial')"
      @playerAttack="attacking('monsterHealth', 'playerSpecial')"
      @special="specialAttack('monsterHealth', 'playerSpecial')"
      @heal="healing()"
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
      progressReverse: true,
      monsterBarFull: false
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
      this.monsterBarFull = false;
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
      let specialGain = Math.floor(Math.random() * 50 + 1);

      if (this.monsterSpecial != 100) {
        this.monsterBarFull = false;
      }

      if (this[e] > regularAtt && !this.gameOver) {
        this[e] -= regularAtt;
      } else if (this[e] <= regularAtt && !this.gameOver) {
        this[e] = 0;
      }

      if (this[a] <= 100 && !this.gameOver && this[a] + specialGain >= 100) {
        this[a] = 100;
      } else if (
        this[a] <= 100 &&
        !this.gameOver &&
        this[a] + specialGain < 100
      ) {
        this[a] += specialGain;
      } 

      if (
        this.monsterSpecial === 100 &&
        !this.gameover &&
        randomMonsterSpecial === 1 &&
        a === "monsterSpecial" &&
        this.monsterBarFull
      ) {
        this.specialAttack("playerHealth", "monsterSpecial");
      }

      if (this.monsterSpecial === 100) {
        this.monsterBarFull = true;
      }

      if (this.monsterHealth <= 0 || this.playerHealth <= 0) {
        this.gameOver = true;
      }
    },
    specialAttack: function(e, a) {
      let specialAtt = Math.floor(Math.random() * 5 + 1) + 15;

      if (!this.gameOver) {
        this[a] = 0;
      }

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
    },
    healing: function() {
      let luckyHeal = Math.floor(Math.random() * 49 + 1);

      if (
        (this.playerHealth < 75 && luckyHeal === 5) ||
        (this.playerHealth > 75 && luckyHeal === 5)
      ) {
        this.playerHealth = 100;
      } else if (
        (this.playerHealth < 75 && luckyHeal != 5) ||
        (this.playerHealth > 75 && luckyHeal != 5)
      ) {
        this.playerHealth += 25;
        this.playerSpecial -= 25;
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
.barDirection {
  direction: rtl;
}
</style>
