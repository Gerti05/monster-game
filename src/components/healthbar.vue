<template>
  <div>
    <v-row>
      <v-col cols="12">
        <h1 class="text-center textColor">{{ player }}</h1>
        <template>
          <div>
            <v-progress-linear :value="health" color="green" height="25">
              <template v-slot="{ value }">
                <strong class="textColor">{{ Math.ceil(value) }}%</strong>
              </template>
            </v-progress-linear>
          </div>
        </template>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  props: ["player", "health"],
};
</script>

<style>
.textColor {
  color: #333;
}
</style>
