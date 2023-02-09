<template lang="">
  <NavBar />
  <div class="container-fluid">
    <div class="row">
      <MenuBar />
      <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
        <router-view></router-view>
      </main>
    </div>
  </div>
</template>

<script>
import NavBar from '@/components/NavBar.vue';
import MenuBar from '@/components/MenuBar.vue';
import { onMounted } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
import { useStore } from 'vuex';

export default {
  name: 'Wrapper',
  components: {
    NavBar,
    MenuBar,
  },
  setup() {
    const router = useRouter();
    const store = useStore();

    onMounted(async () => {
      try {
        const { data } = await axios.get('user');

        await store.dispatch('User/setUser', data);
      } catch (error) {
        await router.push('/login');
      }
    });
  },
};
</script>
