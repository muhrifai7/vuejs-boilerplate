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
                  <span>{{ city }}</span>
                </v-col>
              </v-row>
              <!-- banner -->
              <v-row justify="center" class="mr-1 ml-1">
                <v-img
                  lazy-src="https://picsum.photos/id/11/10/6"
                  aspect-ratio="3.7"
                  src="./assets/images/moscue.jpg"
                >
                  <v-col class="py-4">
                    <v-list-item color="rgba(0, 0, 0, .4)" dark>
                      <v-list-item-content>
                        <v-list-item-title class="title">
                          {{ hours }}
                        </v-list-item-title>
                        <span>Next Prayer</span>
                      </v-list-item-content>
                    </v-list-item>
                  </v-col>
                </v-img>
              </v-row>
              <!-- subheader -->
              <v-row justify="space-between" align="center">
                <v-col align="star" justify="center">
                  <h3>Muharram 1442</h3>
                </v-col>
                <v-col align="end" justify="center">
                  <span>{{ getCurrentDay() }}</span>
                </v-col>
              </v-row>
              <!-- Dates -->
              <v-row class="mr-1 ml-1">
                <v-card elevation="2">
                  <v-list-item>
                    <v-list-item-content
                      align="center"
                      v-for="(item, index) in dates"
                      :key="index"
                      class="mr-4 ml-5"
                    >
                      <v-list-item-title>{{ item.name }}</v-list-item-title>
                      <v-list-item-subtitle>
                        {{ item.date }}
                      </v-list-item-subtitle>
                      <v-list-item-subtitle id="time_index">
                        {{ index + 1 }}
                      </v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>
                </v-card>
              </v-row>
              <!-- prayer time -->
              <div
                v-for="(item, i) in prays_api"
                :key="i"
                class="mt-5 mr-2 ml-2"
              >
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
                            {{ item.icon_left }}
                          </v-icon>
                        </div>
                        <div class="ma-1" id="isActive" justify="center">
                          {{ item.title }}
                        </div>
                      </div>
                      <div align="end" justify="center" class="pray_card ma-2">
                        <div class="ma-1" justify="center" id="isActive">
                          {{ item.time }}
                        </div>
                        <div class="ma-1" justify="center">
                          <v-icon medium color="darken-2">
                            {{ item.icon_right }}
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
          v-for="(item, index) in items"
          :key="index"
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
const axios = require("axios");
export default {
  data: () => ({
    prays: [
      {
        title: "",
        icon_right: "mdi-earth",
        icon_left: "mdi-weather-night-partly-cloudy",
      },
      {
        title: "",
        icon_right: "mdi-earth",
        icon_left: "mdi-weather-night-partly-cloudy",
      },
      {
        title: "",
        icon_right: " mdi-earth",
        icon_left: "mdi-weather-night-partly-cloudy",
      },
      {
        title: "",
        icon_right: "mdi-earth",
        icon_left: "mdi-weather-night-partly-cloudy",
      },
      {
        title: "",
        icon_right: "mdi-earth",
        icon_left: "mdi-weather-night-partly-cloudy",
      },
      {
        title: "",
        icon_right: "mdi-earth",
        icon_left: "mdi-weather-night-partly-cloudy",
      },
      {
        title: "",
        icon_right: "mdi-earth",
        icon_left: "mdi-weather-night-partly-cloudy",
      },
      {
        title: "Isya",
        icon_right: "mdi-earth",
        icon_left: "mdi-weather-night-partly-cloudy",
      },
      {
        title: "",
        icon_right: "mdi-earth",
        icon_left: "mdi-weather-night-partly-cloudy",
      },
    ],
    items: [
      { title: "Home", icon: "mdi-view-dashboard" },
      { title: "Sholat", icon: "mdi-image" },
      { title: "Quran", icon: " mdi-call-split" },
      { title: "Profile", icon: " mdi-dialpad" },
    ],
    dates: [],
    months: [
      "January",
      "February",
      "March",
      "April",
      "May",
      "June",
      "July",
      "August",
      "September",
      "October",
      "November",
      "December",
    ],
    days: ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
    prays_api: [],
    city: "",
    hours: "",
  }),
  methods: {
    getCurrentDay: function() {
      const today = new Date();
      const monthIndex = today.getMonth();
      const monthName = this.months[monthIndex];
      const year = today.getFullYear();
      return monthName + " " + year;
    },
    getOneWeek: function() {
      const today = new Date();
      const monthIndex = today.getMonth();
      const monthName = this.months[monthIndex];
      const year = today.getFullYear();
      let nextweek = new Date(
        today.getFullYear(),
        today.getMonth(),
        today.getDate() + 7
      );
      let getToday = new Date(
        today.getFullYear(),
        today.getMonth(),
        today.getDate()
      );
      let starDate = getToday.getDate();
      let endDate = 7;
      var dates = [];
      for (let i = 1; i < endDate; i++) {
        var date = new Date();
        var thatDay = date.getDate() - i; //Current Date
        date.setDate(thatDay);
        let day = date.getDate();
        let month = date.getMonth() + 1;
        let year = date
          .getFullYear()
          .toString()
          .substr(-2);

        const dayName = this.days[date.getDay()];
        dates.push({
          name: dayName,
          date: day,
        });
      }
      return (this.dates = dates.reverse());
    },
    getPray: async function() {
      let data = await axios
        .get(
          "https://api.pray.zone/v2/times/day.json?city=jakarta&date=2020-12-18"
        )
        .then((response) => response.data.results);
      let prayer_time = data.datetime[0].times;
      let arryTime = Object.entries(prayer_time);
      let resultPray = [];
      for (let [index, item] of arryTime.entries()) {
        resultPray.push({
          title: item[0],
          time: item[1],
          icon_left: this.prays[index].icon_left,
          icon_right: this.prays[index].icon_right,
          is_active: true,
        });
      }
      console.log(arryTime, "ddd");
      this.prays_api = resultPray;
      this.city = data.location.city;
    },
    showHours: function() {
      var date = new Date();
      var am_pm = "AM";
      var hour = date.getHours();
      if (hour >= 12) {
        am_pm = "PM";
      }
      if (hour == 0) {
        hour = 12;
      }
      if (hour > 12) {
        hour = hour - 12;
      }
      if (hour < 10) {
        hour = "0" + hour;
      }

      var minute = date.getMinutes();
      if (minute < 10) {
        minute = "0" + minute;
      }
      var sec = date.getSeconds();
      if (sec < 10) {
        sec = "0" + sec;
      }
      this.hours = `${hour}:${minute}:${sec}`;
    },
  },
  mounted() {
    this.getOneWeek();
    this.getPray();
    setInterval(() => {
      this.showHours();
    }, 1000);
  },
  computed: {},
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

#isActive {
  opacity: 0.6;
}
#time_index {
  font-size: 10px;
}
</style>
