<template>
  <div>
    <Healthbar :player="players[0]" />
    <Healthbar :player="players[1]" />
  </div>
</template>

<script>
import Healthbar from "./healthbar";
export default {
  data: function() {
    return {
      players: ["You", "Monster"],
    };
  },
  components: {
    Healthbar,
  },
};
</script>

<style></style>
