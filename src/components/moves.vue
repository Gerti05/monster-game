<template>
<v-container>
<v-simple-table class="text-center">
    <template v-slot:default>
      <thead>
        <tr>
         
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in monsterMove" :key="item.index">
          <td v-if="item <= 10">Monster attacked for {{ item }} points.</td>
          <td v-if="item > 10">Monster special attacked for {{ item }} points.</td>
        </tr>
      </tbody>
      <tbody>
        <tr v-for="item in playerMove" :key="item.index">
          <td v-if="item <= 10">You attacked for {{ item }} points.</td>
          <td v-if="item > 10">You special attacked for {{ item }} points.</td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
</v-container>
</template>

<script>
  export default {
    props: ["monsterMove", "playerMove"]
  }
</script>

<style>

</style>