<template>
  <div>
    <Healthbar class="inline" :player="players[0]" :health="playerHealth" :special="playerSpecial"/>
    <Healthbar class="inline" :player="players[1]" :health="monsterHealth" :special="monsterSpecial"/>
    <PlayerActions />
  </div>
</template>

<script>
import Healthbar from "./healthbar";
import PlayerActions from "./playerActions"
export default {
  data: function() {
    return {
      players: ["You", "Monster"],
      playerHealth: 65,
      monsterHealth: 40,
      playerSpecial: 0,
      monsterSpecial: 100
    };
  },
  components: {
    Healthbar,
    PlayerActions
  },
};
</script>

<style>
.inline {
  display: inline-block;
  width: 48%;
  margin: 1%;
}
</style>
