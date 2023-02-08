<template lang="">
  <h1>{{ count }}</h1>
  <input type="number" v-model="count" />
</template>

<script lang="ts">
import { ref } from 'vue';
export default {
  setup() {
    const count = ref(0);

    return {
      count,
    };
  },
};
</script>
