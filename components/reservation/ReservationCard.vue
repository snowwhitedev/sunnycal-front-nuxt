<template>
  <div>
    <v-card class="card align-self ma-5 pa-10" outlined tile>
      <div class="activity d-flex flex-row justify-center align-center mt-5">
        <v-icon class="icon-activity">{{ icon }}</v-icon>
      </div>
      <div class="percentage d-flex flex-row justify-center align-center">
        {{ percentage }}<span>%</span>
      </div>
      <div class="text-card text-center align-bottom">{{ activity }}</div>
    </v-card>
  </div>
</template>

<script>
export default {
  name: 'ReservationCard',
  props: {
    activity: {
      type: String,
      default: 'Pool Area'
    },
    icon: {
      type: String,
      default: 'mdi-swim'
    },
    percentage: {
      type: String,
      default: '88'
    }
  }
}
</script>

<style scoped lang="scss">
.card {
  width: 390px;
  height: 542px;
  background: $background-color 0% 0% no-repeat padding-box;
  border-radius: 69px;
  opacity: 1;
  color: #252525;
  font-size: 28px;
  font-weight: bold;
}

.card:hover {
  background: $strong_primary_color 0% 0% no-repeat padding-box;
  box-shadow: 0px 5px 6px #00000040;
  color: white;
}
.activity {
  width: 308px;
  height: 308px;
  background: #ffffff 0% 0% no-repeat padding-box;
  border-radius: 50% !important;
}
.icon-activity {
  width: 105px;
  height: 95px;
  font-size: 130px;
  color: #252525 !important;
  opacity: 1 !important;
}
.percentage {
  width: 130px;
  height: 130px;
  background-color: $strong_primary_color;
  color: white;
  border-radius: 50%;
  border: 10px solid $background-color;
  margin-top: -334px;
  font-size: 36px;
  padding-left: 5px;
}
.percentage span {
  font-size: 20px;
}
.text-card {
  margin-top: 270px;
}
</style>
