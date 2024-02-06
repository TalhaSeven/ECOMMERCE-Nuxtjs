<script setup>
const props = defineProps({
    val: String,
});
</script>

<template>
    <div>
      Test {{ props.val }}
    </div>
  </template>