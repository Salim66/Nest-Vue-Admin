<template>
  <nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow">
    <a href="#" class="navbar-brand col-md-3 col-lg-2 mr-0 px-3">DevZon</a>
    <nav class="my-2 my-md-0 mr-md-3">
      <router-link to="/profile" class="p-2 text-white text-decoration-none">{{
        name
      }}</router-link>
      <router-link
        to="/login"
        class="p-2 text-white text-decoration-none"
        @click.prevent="logout"
        >Logout</router-link
      >
    </nav>
  </nav>
</template>

<script>
import { watch, ref, computed } from 'vue';
import axios from 'axios';
import { useStore } from 'vuex';
export default {
  name: 'NavBar',
  setup() {
    const name = ref('');
    const store = useStore();

    const user = computed(() => store.state.User.user);

    watch(user, () => {
      name.value = user.value.first_name + ' ' + user.value.last_name;
    });

    const logout = async () => {
      await axios.post('logout');
    };

    return {
      name,
      logout,
    };
  },
};
</script>
