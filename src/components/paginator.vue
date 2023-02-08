<template lang="text">
   <nav class="d-flex justify-content-end">
    <ul class="pagination">
      <li class="page-item">
        <a href="#" class="page-link" @click.prevent="prev"> Previous </a>
      </li>
      <li class="page-item">
        <a href="#" class="page-link" @click.prevent="next"> Next </a>
      </li>
    </ul>
  </nav>
</template>
<script lang="ts">
import { ref, SetupContext, watch } from 'vue';
export default {
  name: 'Paginator',
  emits: ['page-changed'],
  props: {
    lastPage: Number,
  },
  setup(props: { lastPage: number }, context: SetupContext) {
    const page = ref(1);

    // when pagination page is change automaticaly run the load match and jump to the next page
    watch(page, () => {
      context.emit('page-changed', page.value);
    });

    const next = () => {
      if (page.value < props.lastPage) {
        page.value++;
      }
    };

    const prev = () => {
      if (page.value > 1) {
        page.value--;
      }
    };

    return {
      next,
      prev,
    };
  },
};
</script>
