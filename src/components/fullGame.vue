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
      <Monster class="avatarBackground" />
    </v-container>
    <PlayerActions />
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
      playerHealth: 65,
      monsterHealth: 40,
      playerSpecial: 0,
      monsterSpecial: 100,
    };
  },
  components: {
    Healthbar,
    PlayerActions,
    Hero,
    Monster,
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
