<template>
  <v-app>
    <!-- Header with top navigation bar -->
    <v-app-bar app color="transparent" elevation="0" class="app-bar-gradient">
      <v-app-bar-nav-icon class="" @click="router.go(-1)">
        <v-icon color="white" size="25">mdi-arrow-left</v-icon>
      </v-app-bar-nav-icon>
      <!-- <v-toolbar-title style="color: #fff">home</v-toolbar-title> -->
    </v-app-bar>
    <v-main>
      <v-layout>
        <v-container class="text-center">
          <v-row class="mb-3 mt-2">
            <v-col>
              <h3 class="text-h4 hidden-md-and-down">
                Select your preferred wallet..
              </h3>
              <h3 class="text-h6 hidden-lg-and-up">
                Select your preferred wallet..
              </h3>
            </v-col>
          </v-row>

          <v-row>
            <v-col
              v-for="wallet in wallets"
              :key="wallet"
              cols="4"
              @click="connectDialogAction"
              class="connect-grid"
            >
              <v-card max-width="200">
                <v-img
                  :height="wallet.height"
                  :src="require(`@/assets${wallet.walletImg}`)"
                  cover
                ></v-img>
              </v-card>
            </v-col>
          </v-row>

          <v-dialog v-model="connectDialog" max-width="320" persistent>
            <v-list class="py-2" color="primary" elevation="12" rounded="lg">
              <v-list-item title="Connecting Wallet...">
                <template v-slot:prepend>
                  <div class="pe-4">
                    <v-icon color="primary" size="x-large"></v-icon>
                  </div>
                </template>

                <template v-slot:append>
                  <v-progress-circular
                    color="primary"
                    indeterminate="disable-shrink"
                    size="16"
                    width="2"
                  ></v-progress-circular>
                </template>
              </v-list-item>
            </v-list>
          </v-dialog>
        </v-container>
      </v-layout>
    </v-main>
  </v-app>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const router = useRouter();
const cryptos = ref([]);

const wallets = ref([
  {
    walletName: "TRUST WALLET",
    walletImg: "/download.png",
  },
  {
    walletName: "METAMASK WALLET",
    walletImg: "/metamask.png",
  },
  {
    walletName: "COINBASE WALLET",
    walletImg: "/coinbase.jpeg",
  },
  {
    walletName: "",
    walletImg: "/binance.jpeg",
  },
  {
    walletName: "HUBOI WALLET",
    walletImg: "/hu.jpeg",
  },
  {
    walletName: "BITPAY WALLET",
    walletImg: "/bitpay.png",
  },
  {
    walletName: "KRAKEN WALLET",
    walletImg: "/krake.png",
  },
  {
    walletName: "SAFEPAL WALLET",
    walletImg: "/safepal.png",
  },
  {
    walletName: "CRYPTO.COM WALLET",
    walletImg: "/crypto.jpeg",
  },
  {
    walletName: "EXODUS WALLET",
    walletImg: "/exo.png",
    height: "100px",
  },
]);
const networkError = ref(null);

const loading = ref(false);
const connectDialog = ref(false);

const connectDialogAction = () => {
  connectDialog.value = true;
  setTimeout(() => {
    loading.value = false;
    router.push({ name: "connect" });
  }, 3000);
};
</script>


<style scoped>
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

.connect-grid {
  cursor: pointer;
}

/* Additional styles can be added here */
</style>