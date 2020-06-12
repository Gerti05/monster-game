<template>
<v-container>
  <v-card class="mx-auto" max-width="344" >
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title class="headline mb-1 text-center"
          >Game Rules</v-list-item-title
        >
      </v-list-item-content>
    </v-list-item>
    <v-card-text>
      <ul>
        <li>
          <div class="text--primary">
            The objective is to send the monsters health to zero.
          </div>
        </li>
        <br>
        <li>
            <div class="text--primary">
            Use the Attack button or Special move button to drop the monsters health.
          </div>
        </li>
        <br>
        <li>
            <div class="text--primary">
            The Special move can do more damage, but can only be used by filling the energy bar.
          </div>
        </li>
        <br>
        <li>
            <div class="text--primary">
            Build up the energy bar by using the regular Attack button.
          </div>
        </li>
        <br>
        <li>
            <div class="text--primary">
            You can also heal yourself by using the Healing button.
          </div>
        </li>
        <br>
        <li>
            <div class="text--primary">
            Healing will cost you 25 energy points.
          </div>
        </li>
        <br>
        <li>
            <div class="text--primary">
            Healing will usually regenerate 25% of your health, but sometimes it can get you back to full power.
          </div>
        </li>
      </ul>
    </v-card-text>
  </v-card>
  </v-container>
</template>

<script>
export default {
};
</script>

<style></style>
