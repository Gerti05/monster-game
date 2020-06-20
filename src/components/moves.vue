<template>
  <v-container>
    <v-card class="mx-auto m" max-width="100%" color="red lighten-1">
      <v-simple-table class="text-center">
        <template v-slot:default>
          <tbody>
            <!-- Takes the items on the moves prop and displays them after a player uses an attack or healing. -->
            <tr
              v-for="item in moves"
              :key="item.index"
              :class="[
                { 'red lighten-1': item.whatMove === 'Regular Attack' },
                { 'orange lighten-1': item.whatMove === 'Special Attack' },
                { 'light-blue lighten-1': item.whatMove === 'Healing' },
              ]"
            >
              <td class="wordColor textSize">
                <span v-if="item.whatMove === 'Regular Attack'"
                  ><Sword class="mr-1"/></span
                ><span v-if="item.whatMove === 'Special Attack'"
                  ><Special class="mr-1"
                /></span>
                <span v-if="item.whatMove === 'Healing'"
                  ><Heal class="mr-1"
                /></span>
                {{ item.whoMoved }} uses {{ item.whatMove }} for
                {{ item.howMuch }} points.
              </td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </v-card>
    <span class="m"></span>
  </v-container>
</template>

<script>
import Sword from "../assets/sword"; //Sword icon that shows what button the player used last turn 
import Special from "../assets/special"; //Bomb icon that shows what button the player used last turn 
import Heal from "../assets/healing"; //Heart icon that shows what button the player used last turn

export default {
  //Moves prop is an array of objects that comes from the fullGame component that tracks how much damage the player or the monster does.
  props: ["moves"],
  components: {
    Sword,
    Special,
    Heal,
  },
};
</script>

<style>
.wordColor {
  color: #fff;
}
</style>
