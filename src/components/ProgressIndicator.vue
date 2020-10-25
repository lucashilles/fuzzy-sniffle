<template>
  <div id="modal">
    <b-spinner label="Loading..." :variant="'light'" style="width: 7rem; height: 7rem"></b-spinner>
  </div>
</template>

<script>
export default {};
</script>

<style>
#modal {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.75);
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: absolute;
  top: 0;
  z-index: 9999;
}
</style>