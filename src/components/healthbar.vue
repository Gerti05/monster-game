<template>
  <div>
    <v-row>
      <v-col cols="12">
        <h1 class="text-center textColor">{{ player }}</h1>
        <template>
          <div>
            <v-progress-linear
              :value="health"
              color="light-green darken-1"
              height="25"
              rounded
              :reverse="reverseBar"
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
            </v-progress-linear>
          </div>
        </template>
      </v-col>
    </v-row>
  </div>
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
</style>
