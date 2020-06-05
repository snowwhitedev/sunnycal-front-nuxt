<template>
  <div>
    <div class="d-flex flex-row mb-10">
      <div class="reservation align-self-start align-center pt-2 pr-12">
        Your reservation
      </div>

      <div class="d-flex justify-center">
        <div class="d-flex flex-row align-center">
          <div class="step selected text-center align-center pt-1 ml-5">1</div>
          <span class="step-text ml-5 align-center">Your Activity </span>
          <hr class="solid self-align-center ml-5 mr-5" />
        </div>

        <div class="d-flex flex-row align-center">
          <div class="step text-center align-center pt-1 ml-5">1</div>
          <span class="step-text ml-5 align-center">Your Crew </span>
          <hr class="solid self-align-center ml-5 mr-5" />
        </div>

        <div class="d-flex flex-row align-center">
          <div class="step text-center align-center pt-1 ml-5 ">1</div>
          <span class="step-text ml-5 align-center">Your Date </span>
          <hr class="solid self-align-center ml-5 mr-5" />
        </div>
      </div>
      <v-icon class="button-primary-icon ml-12" right>mdi-help-circle</v-icon>
      <v-icon class="button-primary-icon second"
        >mdi-close-circle-outline</v-icon
      >
    </div>
    <hr />

    <div class="reservations-title text-center mt-12 mb-10">
      Where would you like to go?
    </div>

    <div class="d-flex flex-row justify-center">
      <ReservationCard
        activity="Pool Area"
        icon="mdi-swim"
        percentage="88"
      ></ReservationCard>
      <ReservationCard
        activity="Beach Area"
        icon="mdi-beach"
        percentage="56"
      ></ReservationCard>
      <ReservationCard
        activity="Tennis Area"
        icon="mdi-tennis"
        percentage="33"
      ></ReservationCard>
    </div>
    <div class="cancel text-center mt-12 mb-10">
      <nuxt-link
        class="cancel text-center align-center justify-center mt-12 mb-10"
        :to="'/' + $route.params.slug"
        >Cancel reservation process</nuxt-link
      >
    </div>
  </div>
</template>

<script>
import ReservationCard from '../../components/reservation/ReservationCard'
export default {
  components: { ReservationCard },
  layout: 'blank',
  validate({ params }) {
    return isNaN(params.slug)
  },
  data() {
    return {
      error: {}
    }
  },
  mounted() {
    // Before loading login page, obtain csrf cookie from the server.
    this.$axios.$get('/sanctum/csrf-cookie')
    console.log(this.$route.params.slug)
  },
  methods: {}
}
</script>
<style scoped lang="scss">
.reservations-title {
  font-family: $title-font-family;
  font-size: 48px;
}
.cancel {
  font-size: 20px;
  text-decoration: underline;
  cursor: pointer;
  color: #252525;
}
.reservation {
  font-size: 24px;
  font-family: $title-font-family;
}
.step {
  color: white;
  border-radius: 50%;
  background-color: #b5b5b5;
  width: 30px;
  height: 30px;
  font-size: 16px;
  font-weight: bold;
}
.step.selected {
  background-color: $strong_primary_color;
}
.activity-hr {
  width: 120px;
}
.step-text {
  font-size: 16px;
  font-weight: bold;
}
.solid {
  border-top: 1px solid #252525;
  width: 150px;
  height: 1px;
}
.button-primary-icon {
  color: $strong_primary_color;
  font-size: 20px;
}
.button-primary-icon.second {
  color: $strong_primary_color;
  padding-left: 5px;
}
</style>
