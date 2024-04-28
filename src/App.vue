<template>
  <v-app id="app">
    <v-main>
      <v-container fill-height fluid>
        <v-row align="center" justify="center">
          <v-col cols="4" md="5">
            <v-card class="pa-4" outlined tile>
              <v-card-title class="headline">You have 50 objects and you have to choose which you
                prefer</v-card-title>
              <v-card-subtitle>You have multi rounds where you have to choose between two different
                clothes</v-card-subtitle>
            </v-card>
          </v-col>
        </v-row>
        <v-row align="center" justify="center">
          <ClothingSelection />
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import ClothingSelection from './components/ClothingSelection.vue'
export default {
  name: 'App',
  components: {
    ClothingSelection
  }
}
</script>

<style>
#app {
  font-family: 'Roboto', sans-serif;
  text-align: center;
  color: #2c3e50;
}

#app img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 50%;
  margin-bottom: 2%;
}
</style>