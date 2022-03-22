<template>
  <button class="pv-button">
    <slot></slot>
  </button>
</template>

<script>
export default {};
</script>

<style lang="scss">
.pv-button {
  font-family: "Rubik", sans-serif;
  font-size: 16px;
  text-transform: uppercase;
  background-color: white;
  box-shadow: rgba(0, 0, 0, 0.25) 0px 25px 50px -12px;
  border: none;
  padding: 8px 16px;
  border-radius: 8px;
  cursor: pointer;
  transition: 0.3s all ease-in-out;
  &:hover {
    background-color: #e7e7e7;
    box-shadow: rgba(0, 0, 0, 0.1) 0px 0px 5px 0px,
      rgba(0, 0, 0, 0.1) 0px 0px 1px 0px;
    transition: 0.3s all ease-in-out;
  }
}
</style>
