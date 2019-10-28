<template>
  <div class="error">
    <h2 class="error__text">It seems we have experienced an error, please try again later.</h2>
  </div>
</template>

<script>
export default {
  name: "error",

  data() {
    return {};
  },
  methods: {}
};
</script>

<style scoped>
.error {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 14rem;
}

.error__text {
  color: black;
  font-size: 2rem;
  padding: 2rem 0;
}
</style>
