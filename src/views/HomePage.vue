<template>
  <ion-page>
    <ion-content>
      <ion-grid>
        <!-- Button to trigger getData method -->
        <ion-row>
          <ion-col size="12" class="ion-text-center" style="text-align: center">
            <ion-button @click="getData" style="text-transform: capitalize">
              GetData
            </ion-button>
          </ion-col>
        </ion-row>
        <!-- Display loading indicator -->
        <ion-row v-if="loading" style="padding: 10px">
          <ion-col size="12" style="text-align: center">Loading...</ion-col>
        </ion-row>
        <!-- Display error message if there is an issue -->
        <ion-row v-if="error" style="color: red; padding: 10px">
          <ion-col size="12" style="text-align: center">{{ error }}</ion-col>
        </ion-row>
        <!-- Header row with Name, Symbol, and Harga USD columns -->
        <ion-row style="border: 2px solid black; padding: 10px">
          <ion-col size="4" style="text-align: center; border-right: 2px solid black">Name</ion-col>
          <ion-col size="4" style="text-align: center; border-right: 2px solid black">Symbol</ion-col>
          <ion-col size="4" style="text-align: center">Harga USD</ion-col>
        </ion-row>
        <!-- Data rows using v-for -->
        <ion-row v-for="item in items" :key="item.id" style="padding: 10px">
          <ion-col size="4" style="text-align: center; border-right: 2px solid black">{{ item.name }}</ion-col>
          <ion-col size="4" style="text-align: center; border-right: 2px solid black">{{ item.symbol }}</ion-col>
          <ion-col size="4" style="text-align: center">{{ item.price_usd }}</ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      items: [],
      error: null,
      loading: false,
    };
  },
  methods: {
    getData() {
      this.loading = true;
      axios
        .get("https://api.coinlore.net/api/tickers/")
        .then((response) => {
          this.items = response.data.data;
          this.error = null;
        })
        .catch((error) => {
          console.log(error);
          if (axios.isAxiosError(error) && !error.response) {
            this.error = "Network error. Please check your internet connection.";
          } else {
            this.error = "Error fetching data. Please try again.";
          }
        })
        .finally(() => {
          this.loading = false;
        });
    },
  },
};
</script>
