<template>
  <div class="card-container">
    <div class="card-title-block">
      <h3 class="card-title"><slot name="title"></slot></h3>
      <button class="card-btn">
        <img src="../../assets/icon-ellipsis.svg" alt="Ellipsis" />
      </button>
    </div>
    <p class="card-time-current"><slot name="current"></slot>hrs</p>
    <div class="card-date-block">
      <span>Last <slot name="date"></slot></span> -
      <span><slot name="previous"></slot>hrs</span>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.card-container {
  position: relative;
  width: 250px;
  background-color: var(--c-dark-blue);
  padding: 25px;
  border-radius: 15px;
  transition: 0.3s ease;
  cursor: pointer;
}
.card-container:hover {
  background-color: var(--c-bit-blue);
  transition: 0.3s ease;
}
.card-title-block {
  display: flex;
  justify-content: space-between;
}
.card-title {
  font-size: 18px;
  font-weight: 500;
}
.card-btn {
  display: flex;
  align-items: center;
}
.card-time-current {
  padding-top: 10px;
  font-size: 58px;
}
.card-date-block {
  font-size: 14px;
  color: var(--c-pale-blue);
}
</style>
