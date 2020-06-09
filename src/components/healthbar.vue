<template>
  <div>
    <v-row>
      <v-col cols="12">
        <h1 class="text-center">{{ player }}</h1>
        <template>
          <div>
            <v-progress-linear :value="health" color="success" height="25">
              <template v-slot="{ value }">
                <strong>{{ Math.ceil(value) }}%</strong>
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

<style></style>
