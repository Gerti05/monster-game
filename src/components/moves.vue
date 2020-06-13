<template>
<v-container>
<v-simple-table dark>
    <template v-slot:default>
      <thead>
        <tr>
          <th class="text-left">Name</th>
          <th class="text-left">Calories</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in monsterMove" :key="item.index">
          <td v-if="item <= 10">Monster attacked for {{ item }} points.</td>
          <td v-if="item > 10">Monster special attacked for {{ item }} points.</td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
</v-container>
</template>

<script>
  export default {
    props: ["monsterMove"]
  }
</script>

<style>

</style>