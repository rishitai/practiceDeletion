<script setup>
const props = defineProps(["label", "data"]);
</script>

<template>
  <p class="handle-node">{{ props.label }}</p>
</template>

<style scoped>
.handle-node {
  background-color: red;
  padding: 10px;
}
</style>
