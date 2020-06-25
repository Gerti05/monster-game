<template>
  <div>
    <!-- Takes in the players health, name, and special as props. -->
    <Healthbar
      class="inline"
      :player="players[0]"
      :health="playerHealth"
      :special="playerSpecial"
      :reverseBar="!progressReverse"
    />
    <!-- Takes in the monsters health, name, and special as props. -->
    <Healthbar
      class="inline barDirection"
      :player="players[1]"
      :health="monsterHealth"
      :special="monsterSpecial"
      :reverseBar="progressReverse"
    />
    <!-- Displays a message depending on who won the game -->
    <v-container class="d-flex justify-space-between text-center textSize">
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
    <!-- Displays the buttons and calles a method depending on which button you click -->
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
    <!-- Rules of the game -->
    <Rules v-if="!gameStarted" />
    <!-- Displays the moves the monster and the player took the pervious turn -->
    <Moves
      v-if="gameStarted"
      @monsterAttack="moveFor('playerHealth')"
      @playerAttack="moveFor('monsterHealth')"
      :moves="movesArray"
    />
    <Footer />
  </div>
</template>

<script>
import Healthbar from "./healthbar"; // Healthbar component
import PlayerActions from "./playerActions"; // Action button component
import Hero from "../assets/hero"; // Hero Avatar
import Monster from "../assets/monster"; // Monster Avatar
import Rules from "./rules"; // Rules component
import Moves from "./moves"; // Previous moves component
import Footer from "./footer"; // Footer component

export default {
  data: function() {
    return {
      players: ["You", "Monster"], // Names on top of healthbar
      playerHealth: 100, // Keeps track of players health
      monsterHealth: 100, // Keeps track of monsters health
      playerSpecial: 0, // Keeps track of players special bar
      monsterSpecial: 0, // Keeps track of monsters special bar
      gameStarted: false, // tracks wheather game has started
      gameOver: false, // Tracks wheather game has ended
      progressReverse: true, // Used to invert the monsters health bar
      monsterBarFull: false, // Checks to see when the monsters special bar gets full
      movesArray: [], // Tracks the previous moves the player and monster did.
    };
  },
  components: {
    Healthbar,
    PlayerActions,
    Hero,
    Monster,
    Rules,
    Moves,
    Footer,
  },
  methods: {
    // When a player clicks the exit button everything reverts back
    exited: function() {
      this.gameStarted = false;
      this.monsterBarFull = false;
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.playerSpecial = 0;
      this.monsterSpecial = 0;
    },
    // When play button is clicked. Both booleans are needed so the action buttons are still displayed even after game is over.
    start: function() {
      this.gameStarted = true;
      this.gameOver = false;
    },

    // Takes in variables to check who is attacking and uses it to lower the health of the opponent, and increase the special bar
    attacking: function(e, a) {
      let regularAtt = Math.floor(Math.random() * 5 + 1); // Randamizes a number between 1 and 5 to use as the player attack
      let randomMonsterSpecial = Math.floor(Math.random() * 2 + 1); // Randamizes a number between 1 and 2 to check if monster can use his special attack.
      let specialGain = Math.floor(Math.random() * 50 + 1); // // Randamizes a number between 1 and 50 to decide how much the special bar should increase by.

      // Checks to see if monster special bar is full
      if (this.monsterSpecial != 100) {
        this.monsterBarFull = false;
      }

      // Checks if attack is less than monster health, and decreases monsters health by regularAtt number, then calls moveFor method.
      if (this[e] > regularAtt && !this.gameOver && e != "playerHealth") {
        this[e] -= regularAtt;
        this.moveFor(regularAtt, "Player", "Regular Attack");
      }
      // Checks if attack is less than players health, and if monster special is not full, and decreases players health by double the regularAtt number, then calls moveFor method.
      else if (
        this[e] > regularAtt * 2 &&
        !this.gameOver &&
        e === "playerHealth" &&
        (!this.monsterBarFull || randomMonsterSpecial != 1)
      ) {
        this[e] -= regularAtt * 2;
        this.moveFor(regularAtt * 2, "Monster", "Regular Attack");
      }
      // Checks if attack is greater than players health, and if monster special is not full, and decreases players health to 0, then calls moveFor method.
      else if (
        this[e] < regularAtt * 2 &&
        !this.gameOver &&
        e === "playerHealth" &&
        (!this.monsterBarFull || randomMonsterSpecial != 1)
      ) {
        this[e] = 0;
        this.moveFor(regularAtt * 2, "Monster", "Regular Attack");
      }
      // Checks if attack is greater than monster health, and decreases monsters health to 0, then calls moveFor method.
      else if (this[e] <= regularAtt && !this.gameOver && e != "playerHealth") {
        this[e] = 0;
        this.moveFor(regularAtt, "Player", "Regular Attack");
      }

      // Increases the special bar based on the specialGain number, or sets it to 100 is the special bar is already almost full.
      if (this[a] <= 100 && !this.gameOver && this[a] + specialGain >= 100) {
        this[a] = 100;
      } else if (
        this[a] <= 100 &&
        !this.gameOver &&
        this[a] + specialGain < 100
      ) {
        this[a] += specialGain;
      }

      // Calls the specialAttack method if the monster special bar is full, and if randomMonsterSpecial number is 1.
      if (
        this.monsterSpecial === 100 &&
        !this.gameOver &&
        randomMonsterSpecial === 1 &&
        a === "monsterSpecial" &&
        this.monsterBarFull
      ) {
        this.specialAttack("playerHealth", "monsterSpecial");
      }

      // If monsters special bar is full it changes monsterBarFull to true.
      if (this.monsterSpecial === 100) {
        this.monsterBarFull = true;
      }

      // Checks if either the monsters or the players health it at 0, and ends the game if it is.
      if (this.monsterHealth <= 0 || this.playerHealth <= 0) {
        this.gameOver = true;
      }
    },

    // Takes in variables to check who is using the special attack, and then resets the special bar variable for that player to 0.
    specialAttack: function(e, a) {
      let specialAtt = Math.floor(Math.random() * 5 + 1) + 15; // Random number between 16 and 20 to use as special attack.

      // Returns the special bar to 0 for whoever is attacking.
      if (!this.gameOver) {
        this[a] = 0;
      }

      // Checks to see if player is attacking, and if the monsters health is greater than specialAtt, and decreases monsters health by specialAtt number, and then calls moveFor method.
      if (specialAtt <= this[e] && !this.gameOver && e != "playerHealth") {
        this[e] -= specialAtt;
        this.moveFor(specialAtt, "Player", "Special Attack");
      }
      // Checks to see if monster is attacking, and if players health is greater than specialAtt, and decreases players health by specialAtt number + 5, and then calls moveFor method.
      else if (
        specialAtt + 5 <= this[e] &&
        !this.gameOver &&
        e === "playerHealth"
      ) {
        this[e] -= specialAtt + 5;
        this.moveFor(specialAtt + 5, "Monster", "Special Attack");
      }
      // If monster is attacking, and players health is lower than specialAtt, it sets players health to 0 and calls moveFor method.
      else if (
        specialAtt + 5 > this[e] &&
        !this.gameOver &&
        e === "playerHealth"
      ) {
        this[e] = 0;
        this.moveFor(specialAtt + 5, "Monster", "Special Attack");
      }
      // If player is attacking, and monsters health is lower than specialAtt, it sets monster health to 0 and calls moveFor method.
      else if (specialAtt > this[e] && !this.gameOver) {
        this[e] = 0;
        this.moveFor(specialAtt, "Player", "Special Attack");
      }

      // Calls the attacking method for the monster if the player is the one that used the special attack.
      if (a === "playerSpecial") {
        this.attacking("playerHealth", "monsterSpecial");
      }

      // Checks to see if the monster or player have a health of 0, and end the game if they do.
      if (this[e] <= 0) {
        this.gameOver = true;
      }
    },

    // This method gets called when player clicks the healing button.
    healing: function() {
      let luckyHeal = Math.floor(Math.random() * 49 + 1); // Randomizes a number between 1 and 49.

      // Sets the players health to 100 if the luckyHeal number is 5
      if (
        (this.playerHealth < 75 && luckyHeal === 5) ||
        (this.playerHealth >= 75 && luckyHeal === 5 && !this.gameOver)
      ) {
        this.playerHealth = 100;
        this.moveFor(100, "Player", "Healing");
      }
      // If players health is less than 75 and luckyHeal is not 5, players health increases by 25, and players special bar is lowered by 50, then moveFor mehtod is called.
      else if (this.playerHealth < 75 && luckyHeal != 5 && !this.gameOver) {
        this.playerHealth += 25;
        this.playerSpecial -= 50;
        this.moveFor(25, "Player", "Healing");
      }
      // If players health is greater than or equal to 75 and luckyHeal is not 5, players health increases to 100, and players special bar is lowered by 50, then moveFor mehtod is called.
      else if (this.playerHealth >= 75 && luckyHeal != 5 && !this.gameOver) {
        this.playerHealth = 100;
        this.playerSpecial -= 50;
        this.moveFor(100, "Player", "Healing");
      }

      // Calls the attacking method for the monster.
      this.attacking("playerHealth", "monsterSpecial");
    },

    // Takes in variables from player and monster and fills an array of objects based on what attack they used, and how much power the attack had.
    moveFor: function(e, a, g) {
      // If moves array length is less than or equal to 4, an object is pushed into it with who attacked, what they attacked with, and how much power it was.
      if (this.movesArray.length <= 4) {
        let obj = {};
        obj["whoMoved"] = a;
        obj["whatMove"] = g;
        obj["howMuch"] = e;
        this.movesArray.push(obj);
      }
      // If moves array length is greater than 4,two of the first objects in the array are erased, and an object is pushed into it with who attacked, what they attacked with, and how much power it was.
      else if (this.movesArray.length > 4) {
        let obj = {};
        obj["whoMoved"] = a;
        obj["whatMove"] = g;
        obj["howMuch"] = e;
        this.movesArray.shift();
        this.movesArray.shift();
        this.movesArray.push(obj);
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
