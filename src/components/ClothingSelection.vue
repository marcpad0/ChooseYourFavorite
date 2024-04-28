<template>
  <v-container class="clothing-selection">
    <v-progress-circular v-if="loading" indeterminate color="primary"></v-progress-circular>
    <v-alert v-else-if="error" type="error">
      Errore durante il caricamento dei dati: {{ error.message }}
    </v-alert>
    <v-row v-else-if="currentProducts.length === 2">
      <v-col cols="12">
        <h2>Round {{ currentRound + 1 }}</h2>
      </v-col>
      <v-col
        v-for="(product, index) in currentProducts"
        :key="index"
        cols="12" sm="6"
      >
        <v-card>
          <v-img v-if="product.images && product.images.length > 0" :src="product.images[0]" :alt="product.title" />
          <v-card-text v-else>Nessuna immagine disponibile</v-card-text>
          <v-card-title>{{ product.title }}</v-card-title>
          <v-card-subtitle>Prezzo: {{ product.price }}</v-card-subtitle>
          <v-card-actions>
            <v-btn color="primary" @click="chooseProduct(product)">Scegli</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <v-row v-else-if="finalWinner" class="winner-row">
      <v-col cols="12">
        <h2>Gioco Finito!</h2>
        <p>IL VINCITORE FINALE è:</p>
      </v-col>
      <v-col cols="12" sm="6">
        <v-card>
          <v-img v-if="finalWinner.images && finalWinner.images.length > 0" :src="finalWinner.images[0]" :alt="finalWinner.title" />
          <v-card-text v-else>Nessuna immagine disponibile</v-card-text>
          <v-card-title>{{ finalWinner.title }}</v-card-title>
          <v-card-subtitle>Prezzo: {{ finalWinner.price }}</v-card-subtitle>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ClothingSelection',
  data() {
    return {
      products: [],
      currentProducts: [],
      currentRound: -1,
      finalWinner: null,
      loading: true,
      error: null,
    };
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
    async fetchProducts() {
      try {
        this.loading = true;
        const response = await axios.get('https://api.escuelajs.co/api/v1/products');
        this.products = response.data;
        this.startNextRound();
      } catch (error) {
        this.error = error;
      } finally {
        this.loading = false;
      }
    },
    chooseProduct(product) {
      const index = this.currentProducts.indexOf(product);
      if (index !== -1) {
        this.currentProducts.splice(index, 1);
        this.products = this.products.filter((p) => p !== product);
      }

      if (this.currentProducts.length === 1) {
        this.startNextRound();
      }
    },
    startNextRound() {
      if (this.products.length >= 2) {
        this.currentProducts = this.products.slice(0, 2);
        this.currentRound++;
      } else if (this.products.length === 1) {
        this.finalWinner = this.products[0];
      }
    },
  },
};
</script>

<style scoped>
.loading-message,
.error-message {
  text-align: center;
  padding: 20px;
}

.product-choice {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.product-image img {
  max-width: 100%;
  height: auto;
}

.product-item {
  margin: 10px;
  text-align: center;
  max-width: 80px; 
}

.no-image {
  height: 150px; 
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
}

.product-title {
  margin-top: 10px;
  font-weight: bold;
}

.product-price {
  margin-bottom: 10px;
}

.winner-row {
  display: flex;
  justify-content: center;
  align-items: center;
}

.choose-button {
  padding: 8px 16px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.game-ended {
  text-align: center;
  padding: 20px;
}
</style>