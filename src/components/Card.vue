<template>
  <div class="pv-card">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "Card",
};
</script>

<style scoped>
.pv-card {
  background-color: white;
  border-radius: 8px;
  padding: 16px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  border: 1px solid #dfdfdf;
}
</style>
