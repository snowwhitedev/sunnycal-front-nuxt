<template>
  <div>
    <v-row>
      <v-col cols="3">
        <div class="logo_container text-center mb-7">
          <v-img class="text-center" src="manursing/logo.png"></v-img>
        </div>
        <v-btn
          nuxt
          :href="'/' + this.$route.params.slug + '/reservation'"
          class="button-primary mb-12"
        >
          New Reservation
          <v-icon class="button-primary-icon" right>mdi-alarm-plus</v-icon>
        </v-btn>

        <v-img class="club-img  mb-12" src="manursing/club.png"></v-img>

        <h2 class="club-title mb-7">{{ $auth.user.data.organization.name }}</h2>

        <p class="club-description mb-8">
          Manursing Island Club is a private family club located on one of the
          most beautiful spots on the Long Island Sound in Rye, New York. Formed
          in 1911, Manursing opened its doors in June, 1912 with Marselis
          Parsons, H.F.G Wey and Cornelius Sewall of Rye and Waldron Williams of
          New York City as its first officers
        </p>

        <div class=" mb-4">
          <v-icon class="icons-address">mdi-email</v-icon> info@club.com
        </div>
        <div class=" mb-4">
          <v-icon class="icons-address">mdi-phone</v-icon> +1 914-967-6400
        </div>
        <div class="club-description mb-4">
          <v-icon class="icons-address">mdi-map-marker</v-icon> 1 S Manursing
          Island, Rye, NY 10580
          <div class="google-maps">
            <a class="link" href="https://maps.google.com"
              >Google maps <v-icon color="primary">mdi-open-in-new</v-icon></a
            >
          </div>
        </div>
      </v-col>
      <v-col cols="9">
        <v-toolbar dense flat>
          <v-spacer></v-spacer>
          <v-btn icon>
            <v-icon large class="toolbar-icons">mdi-help</v-icon>
          </v-btn>
          <v-btn icon>
            <v-icon large class="toolbar-icons">mdi-bell-ring</v-icon>
          </v-btn>

          <v-menu offset-y>
            <template v-slot:activator="{ on }">
              <v-btn icon v-on="on">
                <v-icon large class="toolbar-icons">mdi-account</v-icon>
              </v-btn>
            </template>
            <v-list>
              <v-list-item
                v-for="(item, index) in account_actions"
                :key="index"
                @click="item.function"
              >
                <v-list-item-title>{{ item.name }}</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>

          <v-btn icon>
            <v-icon large class="toolbar-icons">mdi-alarm-plus</v-icon>
          </v-btn>
        </v-toolbar>
        <h1 class="club-title">{{ $auth.user.data.organization.name }}</h1>
        <div class="family-title">
          Welcomes the {{ $auth.user.data.name }} Family
        </div>

        <v-card class="calendar-box pa-6">
          <div class="reservations-title">Your reservations</div>
          <v-sheet height="600">
            <v-calendar
              ref="calendar"
              :now="today"
              :value="today"
              :events="events"
              color="primary"
              type="week"
              @click:event="showEvent"
            ></v-calendar>
          </v-sheet>
          <v-menu
            v-model="selectedOpen"
            :close-on-content-click="false"
            :activator="selectedElement"
            offset-x
          >
            <v-card color="grey lighten-4" min-width="350px" flat>
              <v-toolbar :color="selectedEvent.color" dark>
                <v-btn icon>
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>

                <v-toolbar-title v-html="selectedEvent.name"></v-toolbar-title>
                <v-spacer></v-spacer>
                <v-btn icon>
                  <v-icon>mdi-dots-vertical</v-icon>
                </v-btn>
              </v-toolbar>
              <v-card-text>
                <span v-html="selectedEvent.details"></span>
              </v-card-text>
              <v-card-actions>
                <v-btn text color="secondary" @click="selectedOpen = false">
                  Cancel
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-menu>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  layout: 'organization',
  middleware: 'auth',
  validate({ params }) {
    return isNaN(params.slug)
  },
  link: [
    {
      rel: 'stylesheet',
      href:
        'https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600&display=swap'
    }
  ],
  data: () => ({
    selectedEvent: {},
    selectedElement: null,
    selectedOpen: false,
    today: '2020-05-25',
    events: [
      {
        name: 'Pool',
        start: '2020-05-25 09:00',
        end: '2020-05-25 10:00'
      },
      {
        name: 'Tennis',
        start: '2020-05-25 18:00'
      }
    ],
    account_actions: [
      {
        name: 'Log Out',
        function: 'logout'
      }
    ]
  }),
  mounted() {
    this.$refs.calendar.scrollToTime('08:00')
  },
  methods: {
    viewDay({ date }) {
      this.focus = date
      this.type = 'day'
    },
    getEventColor(event) {
      return event.color
    },
    setToday() {
      this.focus = this.today
    },
    prev() {
      this.$refs.calendar.prev()
    },
    next() {
      this.$refs.calendar.next()
    },
    showEvent({ nativeEvent, event }) {
      const open = () => {
        this.selectedEvent = event
        this.selectedElement = nativeEvent.target
        setTimeout(() => (this.selectedOpen = true), 10)
      }

      if (this.selectedOpen) {
        this.selectedOpen = false
        setTimeout(open, 10)
      } else {
        open()
      }

      nativeEvent.stopPropagation()
    },
    updateRange({ start, end }) {
      const events = []

      const min = new Date(`${start.date}T00:00:00`)
      const max = new Date(`${end.date}T23:59:59`)
      const days = (max.getTime() - min.getTime()) / 86400000
      const eventCount = this.rnd(days, days + 20)

      for (let i = 0; i < eventCount; i++) {
        const allDay = this.rnd(0, 3) === 0
        const firstTimestamp = this.rnd(min.getTime(), max.getTime())
        const first = new Date(firstTimestamp - (firstTimestamp % 900000))
        const secondTimestamp = this.rnd(2, allDay ? 288 : 8) * 900000
        const second = new Date(first.getTime() + secondTimestamp)

        events.push({
          name: this.names[this.rnd(0, this.names.length - 1)],
          start: this.formatDate(first, !allDay),
          end: this.formatDate(second, !allDay),
          color: this.colors[this.rnd(0, this.colors.length - 1)]
        })
      }

      this.start = start
      this.end = end
      this.events = events
    },
    nth(d) {
      return d > 3 && d < 21
        ? 'th'
        : ['th', 'st', 'nd', 'rd', 'th', 'th', 'th', 'th', 'th', 'th'][d % 10]
    },
    rnd(a, b) {
      return Math.floor((b - a + 1) * Math.random()) + a
    },
    formatDate(a, withTime) {
      return withTime
        ? `${a.getFullYear()}-${a.getMonth() +
            1}-${a.getDate()} ${a.getHours()}:${a.getMinutes()}`
        : `${a.getFullYear()}-${a.getMonth() + 1}-${a.getDate()}`
    }
  },
  async logout() {
    await this.$auth.logout()

    this.$router.push({
      path: '/login'
    })
  }
}
</script>
<style lang="scss" scoped>
.logo_container {
  width: 350px;
  height: 180px;
  background: #ffffff 0% 0% no-repeat padding-box;
  box-shadow: 0px 5px 8px #00000029;
  border-radius: 10px;
  padding: 49px 33px;
}
.club-title {
  font-family: $title-font-family;
  font-size: 28px;
  font-weight: 400;
}

.club-img {
  border-radius: 10px;
  width: 350px;
  height: 260px;
}
.family-title {
  font-family: $title-font-family;
  font-size: 42px;
  font-weight: 500;
}
h1 {
  margin-top: 30px;
}
.calendar-box {
  background: #ffffff 0% 0% no-repeat padding-box;
  box-shadow: 0px 3px 6px #00000029;
  margin-top: 140px;
}
.reservations-title {
  color: $strong_primary_color;
  font-family: $title-font-family;
  font-weight: 600;
  font-size: 20px;
  margin-bottom: 23px;
}
.top-reservation {
  background-color: $strong_primary_color !important;
  color: #ffffff !important;
}
.toolbar-icons {
  color: $strong_primary_color !important;
}
.club-description {
  max-width: 364px;
}
.icons-address {
  color: $strong_primary_color !important;
  margin-right: 22px;
}
.google-maps {
  margin-left: 50px;
}
</style>
