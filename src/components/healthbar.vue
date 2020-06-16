<template>
  <v-container>
    <div>
      <v-row>
        <v-col cols="12">
          <h1 class="text-center textColor textSize2">{{ player }}</h1>
          <template v-if="health > 25">
            <div>
              <v-progress-linear
                :value="health"
                height="25"
                rounded
                :reverse="reverseBar"
                color="light-green darken-1"
              >
                <template v-slot="{ value }">
                  <strong class="textColor">{{ Math.ceil(value) }}%</strong>
                </template>
              </v-progress-linear>
            </div>
          </template>
          <template v-else>
            <div>
              <v-progress-linear
                :value="health"
                height="25"
                rounded
                :reverse="reverseBar"
                color="red"
              >
                <template v-slot="{ value }">
                  <strong class="textColor">{{ Math.ceil(value) }}%</strong>
                </template>
              </v-progress-linear>
            </div>
          </template>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="6" class="pt-0">
          <template>
            <div>
              <v-progress-linear
                :value="special"
                color="orange lighten-1"
                height="15"
                rounded
                :reverse="reverseBar"
              >
                <template v-slot="{ value }">
                  <strong class="textColor textSize"
                    >{{ Math.ceil(value) }}/100</strong
                  >
                </template>
              </v-progress-linear>
            </div>
          </template>
        </v-col>
      </v-row>
    </div>
  </v-container>
</template>

<script>
export default {
  props: ["player", "health", "special", "reverseBar"],
};
</script>

<style>
.textColor {
  color: #333;
}
@media only screen and (max-width: 600px) {
  .textSize {
    font-size: 0.7em !important;
  }
  .textSize2 {
    font-size: 1em !important;
  }
}
</style>
