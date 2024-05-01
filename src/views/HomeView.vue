<template>
  <v-app>
    <!-- Header with top navigation bar -->
    <v-app-bar app color="transparent" elevation="0" class="app-bar-gradient">
      <!-- <v-app-bar-nav-icon
        @click="drawer = !drawer"
        color="white"
      ></v-app-bar-nav-icon> -->
      <v-toolbar-title style="color: #fff">Connect</v-toolbar-title>

      <!-- <v-spacer></v-spacer> -->
      <!-- <div>
        <v-btn variant="outlined" color="white">Connect</v-btn>
      </div> -->
    </v-app-bar>
    <v-main class="">
      <!-- Hero section -->
      <!-- Hero section -->
      <v-container fluid class="hero-section">
        <div class="pa-10">
          <div class="hero-content text-center mb-6">
            <h1 class="display-1 text-white font-weight-bold text-h4">
              Decentralized. Secure. Yours.
            </h1>
            <p class="subtitle-1 text-white text-h5">
              Take control of your online identity with our decentralized
              website.
            </p>
          </div>

          <div class="d-flex justify-center">
            <div class="d-flex flex-row">
              <div class="ma-2 pa-2 flex-grow-1 flex-shrink-0">
                <v-btn
                  rounded="0"
                  size="x-large"
                  block
                  @click="load"
                  :loading="loading"
                  >CONNECT</v-btn
                >
              </div>
              <div class="ma-2 pa-2">
                <v-btn rounded="0" size="x-large" block>MIGRATE</v-btn>
              </div>
            </div>
          </div>

          <!-- 
          <v-row justify="center mt-4">
            <v-col md="6" sm="6" lg="6">
              <v-btn
                rounded="0"
                size="x-large"
                block
                transparent
                style="color: #fff; border: 2px solid #ef4444"
                variant="outlined"
                >CLAIM</v-btn
              >
            </v-col>
            <v-col md="6" sm="6" lg="6">
              <v-btn
                rounded="0"
                size="x-large"
                block
                variant="outlined"
                style="color: #fff; border: 2px solid #ef4444"
                >VERIFY</v-btn
              >
            </v-col>
            <v-col md="6" sm="6" lg="6">
              <v-btn
                rounded="0"
                size="x-large"
                block
                variant="outlined"
                style="color: #fff; border: 2px solid #ef4444"
                >SYNC</v-btn
              >
            </v-col>
            <v-col md="6" sm="6" lg="6">
              <v-btn
                rounded="0"
                size="x-large"
                block
                variant="outlined"
                style="color: #fff; border: 2px solid #ef4444"
                >MIGRATE</v-btn
              >
            </v-col>
          </v-row> -->
        </div>
      </v-container>

      <v-container fluid>
        <!-- <div class="hero-section pa-10">
          <div class="text-center">
            <h1 class="display-1 text-white font-weight-bold text-h4">
              Decentralized. Secure. Yours.
            </h1>

            <p class="subtitle-1 text-white text-h5">
              Take control of your online identity with our decentralized
              website.
            </p>

            <v-row justify="center mt-4">
              <v-col md="6" sm="6" lg="6">
                <v-btn
                  rounded="0"
                  size="x-large"
                  block
                  variant="outlined"
                  style="color: #fff; border: 2px solid #ef4444"
                  >CLAIM</v-btn
                >
              </v-col>
              <v-col md="6" sm="6" lg="6">
                <v-btn
                  rounded="0"
                  size="x-large"
                  block
                  variant="outlined"
                  style="color: #fff; border: 2px solid #ef4444"
                  >VERIFY</v-btn
                >
              </v-col>
              <v-col md="6" sm="6" lg="6">
                <v-btn
                  rounded="0"
                  size="x-large"
                  block
                  variant="outlined"
                  style="color: #fff; border: 2px solid #ef4444"
                  >SYNC</v-btn
                >
              </v-col>
              <v-col md="6" sm="6" lg="6">
                <v-btn
                  rounded="0"
                  size="x-large"
                  block
                  variant="outlined"
                  style="color: #fff; border: 2px solid #ef4444"
                  >MIGRATE</v-btn
                >
              </v-col>
            </v-row>
            <v-row> </v-row>
          </div>
        </div> -->

        <v-row>
          <div class="pa-10" v-if="networkError">
            <p class="">{{ networkError }}</p>
          </div>

          <v-col
            v-for="crypto in cryptos"
            :key="crypto.id"
            cols="12"
            md="4"
            sm="4"
            lg="4"
          >
            <v-card>
              <v-item-group selected-class="bg-primary">
                <v-list class="ma-1 mb-0" lines="three" item-props>
                  <v-item>
                    <v-list-item class="mb-4">
                      <template v-slot:prepend>
                        <v-avatar>
                          <v-img :src="crypto.image"></v-img>
                        </v-avatar>
                      </template>
                      <v-list-item-content>
                        <v-list-item-title
                          class="mb-1 font-styled"
                          style="font-size: 18px"
                        >
                          {{ crypto.name }}
                        </v-list-item-title>
                        <v-list-item-subtitle class="text-styled mt-2">
                          Volume
                        </v-list-item-subtitle>

                        <!-- <v-list-item-subtitle class="text-styled caption">
                        maximum amount : &#x20A6;{{ service.maximum_amount }}
                      </v-list-item-subtitle> -->
                        <!-- <v-list-item-subtitle
                        class="text-styled text-end caption"
                      >
                        minimum: {{ service.minimum_amount }}
                      </v-list-item-subtitle> -->
                      </v-list-item-content>
                      <template v-slot:append>
                        <v-list-item-content>
                          <v-list-item-title class="font-styled mb-2">
                            ${{ crypto.current_price }}
                          </v-list-item-title>
                          <v-list-item-subtitle
                            class="text-green text-styled text-end caption"
                          >
                            ${{ crypto.total_volume }}
                          </v-list-item-subtitle>
                        </v-list-item-content>
                      </template>
                    </v-list-item>
                  </v-item>
                </v-list>
              </v-item-group>

              <!-- <v-card-title>{{ crypto.name }}</v-card-title>
          <v-card-text>
            <div><strong>Current Price:</strong> ${{ bitcoinPrice }}</div>
            <div><strong>Rate Change:</strong> {{ bitcoinRateChange }}</div>
          </v-card-text> -->
            </v-card>
            <!-- <v-card>
          <v-card-title>{{ crypto.name }}</v-card-title>
         
        </v-card> -->
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const router = useRouter();
const cryptos = ref([]);
const networkError = ref(null);

const loading = ref(false);

const load = () => {
  loading.value = true;
  setTimeout(() => {
    loading.value = false;
    router.push({ name: "wallets" });
  }, 3000);
};

const getPrices = () => {
  return new Promise((resolve, reject) => {
    try {
      const response = axios.get(
        "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids=bitcoin,binancecoin,ethereum,cardano,polygon,tether,xrp,solana,dogecoin,litecoin"
      );
      resolve(response);
    } catch (error) {
      reject(error);
    }
  });
};

onMounted(async () => {
  setTimeout(() => {
    getPrices()
      .then((response) => (cryptos.value = response.data))
      .catch((err) => {
        networkError.value = "Cannot Fetch Prices At The Moment....";
      });
  }, 3000);
});
</script>


<style scoped>
/* Header styles */
.hero-section {
  /* background: linear-gradient(to right, black, #ef4444),
    url("../assets/dappsBg.31224a1c4151a363901b.jpeg"); */
  background-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.5),
      rgba(0, 0, 0, 0.8)
    ),
    url("../assets/dappsBg.31224a1c4151a363901b.jpeg");

  background-size: cover;
  background-repeat: no-repeat;
  background-attachment: fixed; /* Optional: Fixed background */
  padding: 40px 0;
  height: 500px; /* Adjust as needed */
}

/* Align text to center */
.text-center {
  text-align: center;
}

.app-bar-gradient {
  background: linear-gradient(
    to right,
    black,
    #ef4444
  ); /* Gradient from left black to right #ef4444 */
}

.app {
}

/* Additional styles can be added here */
</style>