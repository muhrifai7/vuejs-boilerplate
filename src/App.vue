<template>
  <v-app id="inspire">
    <v-main class="grey lighten-3 mb-3">
      <v-container>
        <v-row align="center" justify="center">
          <v-col cols="6">
            <v-sheet min-height="70vh" rounded="lg" class="pa-4">
              <!-- header -->
              <v-row justify="space-between" align="center">
                <v-col align="star" justify="center">
                  <h3>Waktu Sholat</h3>
                </v-col>
                <v-col align="end" justify="center">
                  <span>Jakarta Pusat</span>
                </v-col>
              </v-row>
              <!-- banner -->
              <v-row justify="center" class="mr-1 ml-1">
                <v-img
                  lazy-src="https://picsum.photos/id/11/10/6"
                  aspect-ratio="3.7"
                  src="./assets/images/moscue.jpg"
                ></v-img>
              </v-row>
              <!-- subheader -->
              <v-row justify="space-between" align="center">
                <v-col align="star" justify="center">
                  <h3>Muharram 1442</h3>
                </v-col>
                <v-col align="end" justify="center">
                  <span>September 2020</span>
                </v-col>
              </v-row>
              <!-- Date -->
              <v-row class="mr-1 ml-1">
                <v-card elevation="2">
                  <v-list-item three-line>
                    <v-list-item-content
                      align="center"
                      v-for="(item, index) in dates"
                      :key="item"
                      class="mr-3 ml-3"
                    >
                      <v-list-item-title>{{ item.name }}</v-list-item-title>
                      <v-list-item-subtitle>
                        {{ item.date }}
                      </v-list-item-subtitle>
                      <v-list-item-subtitle>
                        {{ index }}
                      </v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>
                </v-card>
              </v-row>
              <!-- prayer time -->
              <div v-for="(item, i) in prays" :key="i" class="mt-5 mr-2 ml-2">
                <!-- <v-col> -->
                <v-hover>
                  <v-card color="#ecf0f1" elevation="0" rounded="xl">
                    <v-row justify="space-between" class="mr-2 ml-2">
                      <div
                        align="start"
                        justify="center"
                        class="pray_card ma-2"
                      >
                        <div class="ma-1" justify="center">
                          <v-icon medium color="darken-2">
                            {{ item.icon }}
                          </v-icon>
                        </div>
                        <div class="ma-1" justify="center">tesadsadst</div>
                      </div>
                      <div align="end" justify="center" class="pray_card ma-2">
                        <div class="ma-1" justify="center">tesadsadst</div>
                        <div class="ma-1" justify="center">
                          <v-icon medium color="darken-2">
                            {{ item.icon }}
                          </v-icon>
                        </div>
                      </div>
                    </v-row>
                  </v-card>
                </v-hover>
                <!-- </v-col> -->
              </div>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
    <v-footer
      class="justify-center pl-0"
      align="center"
      justify="center"
      inset
      app
    >
      <v-tabs centered color="grey darken-1">
        <v-tab
          v-for="item in items"
          :key="item"
          @click="currentTab = index"
          class="pr-10 pl-10"
        >
          <v-icon large color="darken-2">
            {{ item.icon }}
          </v-icon>
          {{ item.title }}
        </v-tab>
      </v-tabs>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    prays: [
      { title: "Subuh", icon: "mdi-earth" },
      { title: "Terbit", icon: "mdi-weather-night-partly-cloudy" },
      { title: "Dzuhur", icon: " mdi-weather-partly-cloudy" },
      { title: "Ashar", icon: " mdi-dialpad" },
      { title: "Magrib", icon: " mdi-dialpad" },
      { title: "Isya", icon: " mdi-dialpad" },
    ],
    items: [
      { title: "Home", icon: "mdi-view-dashboard" },
      { title: "Sholat", icon: "mdi-image" },
      { title: "Quran", icon: " mdi-call-split" },
      { title: "Profile", icon: " mdi-dialpad" },
    ],
    dates: [
      { name: "Sen", date: 10 },
      { name: "Sel", date: 11 },
      { name: "Rab", date: 12 },
      { name: "Kam", date: 13 },
      { name: "Jum", date: 14 },
      { name: "Sab", date: 15 },
      { name: "Ming", date: 16 },
    ],
  }),
};
</script>

<style lang="scss">
.v-tab {
  display: flex;
  flex-direction: column;
}
* {
  color: #3498db;
}
.pray_card {
  display: flex;
  flex-direction: row;
  padding: 10px;
}
.active {
  background: rgb(17, 128, 17);
}
</style>
