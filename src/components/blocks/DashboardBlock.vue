<template>
  <div class="wrapper">
    <div class="dashboard-container">
      <section class="dashboard-section">
        <div class="card-setting">
          <div class="card-profile">
            <img
              class="card-profile-img"
              src="../../assets/image-jeremy.png"
              alt="Profile image"
            />
            <div>
              <p class="card-profile-paragraph">Report for</p>
              <h2 class="card-profile-name">Jeremy Robson</h2>
            </div>
          </div>
          <div class="btn-group">
            <button
              :class="{ 'active-btn': activeBtn == 'daily' }"
              @click="getCurrentDate('daily')"
            >
              Daily
            </button>
            <button
              :class="{ 'active-btn': activeBtn == 'weekly' }"
              @click="getCurrentDate('weekly')"
            >
              Weekly
            </button>
            <button
              :class="{ 'active-btn': activeBtn == 'monthly' }"
              @click="getCurrentDate('monthly')"
            >
              Monthly
            </button>
          </div>
        </div>
        <div v-for="value in data" :key="value.title">
          <div :class="getCurrentColor(value.title)" class="card-background">
            <img
              class="card-img"
              :src="getCurrentLink(value.title)"
              alt="Card"
            />
          </div>
          <BaseCard v-if="currentDate === 'daily'">
            <template v-slot:title>{{ value.title }}</template>
            <template v-slot:current>{{
              value.timeframes.daily.current
            }}</template>
            <template v-slot:date>Day</template>
            <template v-slot:previous>{{
              value.timeframes.daily.previous
            }}</template>
          </BaseCard>
          <BaseCard v-if="currentDate === 'weekly'">
            <template v-slot:title>{{ value.title }}</template>
            <template v-slot:current>{{
              value.timeframes.weekly.current
            }}</template>
            <template v-slot:date>Week</template>
            <template v-slot:previous>{{
              value.timeframes.weekly.previous
            }}</template>
          </BaseCard>
          <BaseCard v-if="currentDate === 'monthly'">
            <template v-slot:title>{{ value.title }}</template>
            <template v-slot:current>{{
              value.timeframes.monthly.current
            }}</template>
            <template v-slot:date>Month</template>
            <template v-slot:previous>{{
              value.timeframes.monthly.previous
            }}</template>
          </BaseCard>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { ref, onMounted } from "vue";
export default {
  setup() {
    // Variables
    const data = ref();
    const currentDate = ref("weekly");
    const activeBtn = ref();

    // Data Rendering
    function fetchData() {
      axios.get("data/data.json").then((responce) => {
        data.value = responce.data;
      });
    }

    // Get Current Color
    function getCurrentColor(cardTitle) {
      cardTitle = cardTitle.toLowerCase().replace(/ /g, "-");

      return `card-${cardTitle}`;
    }

    // Get Current Link
    function getCurrentLink(cardTitle) {
      cardTitle = cardTitle.toLowerCase().replace(/ /g, "-");
      let filename = `images/icon-${cardTitle}.svg`;

      return filename;
    }

    // Get Current Date
    function getCurrentDate(date) {
      currentDate.value = date;
      activeBtn.value =
        activeBtn.value === currentDate.value ? null : currentDate.value;
    }

    onMounted(() => {
      activeBtn.value =
        activeBtn.value === currentDate.value ? null : currentDate.value;

      fetchData();
    });

    return {
      data,
      currentDate,
      activeBtn,
      fetchData,
      getCurrentLink,
      getCurrentColor,
      getCurrentDate,
    };
  },
};
</script>

<style scoped>
.dashboard-container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.dashboard-section {
  display: grid;
  grid-gap: 25px;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: auto;
}
.card-setting {
  grid-row-start: 1;
  grid-row-end: 3;

  display: flex;
  flex-direction: column;
  justify-content: space-between;
  background-color: var(--c-dark-blue);
  border-radius: 15px;
}
.card-profile {
  background-color: var(--c-blue);
  border-radius: 15px;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: space-between;
  padding: 25px;
}
.card-profile-img {
  width: 100px;
  border: 3px solid var(--c-white);
  border-radius: 50px;
}
.card-profile-paragraph {
  font-size: 14px;
  font-weight: 400;
  color: var(--c-pale-blue);
}
.card-profile-name {
  line-height: 1.4;
  width: 200px;
  font-size: 38px;
  font-weight: 300;
}
.btn-group {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 25px;
}
.btn-group button {
  color: var(--c-desaturated-blue);
  font-family: "Rubik", sans-serif;
  font-size: 16px;
  font-weight: 400;
  margin: 5px 0;
  transition: 0.3s ease;
}
.btn-group button:hover {
  color: var(--c-white);
  transition: 0.3s ease;
}
.active-btn {
  color: var(--c-white) !important;
  transition: 0.3s ease;
}
.card-background {
  margin-top: -30px;
  position: relative;
  top: 30px;
  border-radius: 15px;
}
.card-img {
  position: relative;
  left: 160px;
}
/* CARD COLORS */
.card-work {
  background-color: var(--c-light-orange);
}
.card-play {
  background-color: var(--c-soft-blue);
}
.card-study {
  background-color: var(--c-light-red);
}
.card-exercise {
  background-color: var(--c-lime-green);
}
.card-social {
  background-color: var(--c-violet);
}
.card-self-care {
  background-color: var(--c-soft-orange);
}

@media only screen and (max-width: 1100px) {
  .dashboard-container {
    margin: 50px 0;
  }
  .dashboard-section {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media only screen and (max-width: 600px) {
  .dashboard-section {
    grid-template-columns: repeat(1, 1fr);
  }
}
</style>
