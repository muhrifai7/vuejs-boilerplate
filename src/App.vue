<template>
  <v-app id="inspire">
    <v-main class="grey lighten-3 mb-3">
      <v-container>
        <v-row align="center" justify="center">
          <v-col>
            <v-sheet rounded="lg" class="pa-4">
              <v-row justify="center">
                <v-dialog v-model="dialog" persistent max-width="290">
                  <template
                    v-slot:activator="{ on, attrs }"
                    v-if="showNotification"
                  >
                    <v-btn color="primary" dark v-bind="attrs" v-on="on">
                      <v-badge
                        :content="messages"
                        :value="messages"
                        color="red"
                        overlap
                      >
                        <v-icon medium color="white">
                          mdi-chat-outline
                        </v-icon>
                      </v-badge>
                    </v-btn>
                  </template>
                  <v-card>
                    <v-card-title>
                      Sudah Waktu Masuk {{ commingPray.name }}
                    </v-card-title>
                    <v-card-text>Sholat DuluYuk!.</v-card-text>
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn
                        color="green darken-1"
                        text
                        @click="dialog = false"
                      >
                        Batal
                      </v-btn>
                      <v-btn
                        color="green darken-1"
                        text
                        @click="[(dialog = false), (messages = 0)]"
                      >
                        Baik
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
              </v-row>
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
                        <p>
                          <span>Next Prayer</span
                          >{{ ` ${commingPray.name}   ${commingPray.hour}` }}
                        </p>
                      </v-list-item-content>
                    </v-list-item>
                  </v-col>
                </v-img>
              </v-row>
              <!-- subheader -->
              <v-row justify="space-between" align="center">
                <v-col align="star" justify="center">
                  <h3>Muharram {{ hijri }}</h3>
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
                    <v-row justify="space-between" class="mr-2 ml-2 isActive">
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
                        <div class="ma-1" justify="center">
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
const moment = require("moment");
export default {
  data: () => ({
    isMobile: false,
    isActive: true,
    dialog: false,
    showNotification: true,
    messages: 1,
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
    hijri: "",
    commingPray: { name: "", hour: "" },
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
      let dateParams = moment().format("YYYY-MM-DD");
      // let data = await axios
      //   .get(
      //     `https://api.pray.zone/v2/times/day.json?city=jakarta&date=2020-12-18`
      //   )
      //   .then((response) => response.data.results);
      // console.log(data, "dataa");
      let data = {
        datetime: [
          {
            times: {
              Asr: "15:17",
              Dhuhr: "11:49",
              Fajr: "04:18",
              Imsak: "04:08",
              Isha: "19:16",
              Maghrib: "18:18",
              Midnight: "23:11",
              Sunrise: "05:34",
              Sunset: "18:04",
            },
            date: {
              timestamp: 1608249600,
              gregorian: "2020-12-19",
              hijri: "1442-05-03",
            },
          },
        ],
        location: {
          latitude: 48.85661315917969,
          longitude: 2.352221965789795,
          elevation: 36.0,
          city: "Jakarta",
          country: "Republic of Indonesia",
          country_code: "FR",
          timezone: "Europe/Paris",
          local_offset: 1.0,
        },
      };
      let hijri = data.datetime[0].date.hijri;
      this.hijri = hijri.split("-")[0];
      let prayer_time = data.datetime[0].times;
      let arryTime = Object.entries(prayer_time);

      let hourPray = [];
      let resultPray = [];
      for (let [index, item] of arryTime.entries()) {
        hourPray.push(item[1]);
        resultPray.push({
          title: item[0],
          time: item[1],
          icon_left: this.prays[index].icon_left,
          icon_right: this.prays[index].icon_right,
          is_active: true,
        });
      }

      let myEnd = "";
      let myStart = moment().format("HH:mm");
      let result = [];
      for (let v of hourPray.sort()) {
        var beginningTime = moment(myStart, "h:mm");
        var endTime = moment(v, "h:mm");
        if (beginningTime.isBefore(endTime) == true) {
          result.push(v);
        }
      }
      let finalRes = result[0];

      const obj = {
        id: 1,
        name: "Den",
      };

      function getKeyByValue(obj, value) {
        return Object.entries(obj).find(([, name]) => value === name);
      }

      const [key] = getKeyByValue(prayer_time, finalRes);
      this.commingPray = { name: key, hour: finalRes };
      this.prays_api = resultPray;
      this.city = data.location.city;
    },
    showHours: function() {
      let nowDate = moment().format("DD/MM/YYYY hh:mm:ss");
      let thenDate = moment().format("DD/MM/YYYY");
      var then = `${thenDate} ${this.commingPray.hour}`;
      let result = moment
        .utc(
          moment(then, "DD/MM/YYYY HH:mm:ss").diff(
            moment(nowDate, "DD/MM/YYYY HH:mm:ss")
          )
        )
        .format("HH:mm:ss");
      if (result === "00:00:00") {
        this.messages++;
        this.showNotification = true;
        this.getPray();
      }
      this.hours = result;
    },
    onResize() {
      this.isMobile = window.innerWidth < 600;
    },
  },
  mounted() {
    console.log(this.$vuetify.breakpoint.width);
    this.getOneWeek();
    this.getPray();
    setInterval(() => {
      this.showHours();
    }, 1000);
    this.onResize();

    window.addEventListener("resize", this.onResize, { passive: true });
  },
  computed: {
    height() {
      switch (this.$vuetify.breakpoint.name) {
        case "xs":
          return 220;
        case "sm":
          return 400;
        case "md":
          return 500;
        case "lg":
          return 600;
        case "xl":
          return 800;
      }
    },
  },
  beforeDestroy() {
    if (typeof window === "undefined") return;

    window.removeEventListener("resize", this.onResize, { passive: true });
  },
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

.isActive {
  opacity: 0.6;
}

#time_index {
  font-size: 10px;
}
</style>
