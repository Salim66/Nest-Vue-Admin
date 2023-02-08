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
import { onMounted, ref } from 'vue';
import axios from 'axios';
export default {
  name: 'NavBar',
  setup() {
    const name = ref('');

    onMounted(async () => {
      const { data } = await axios.get('user');

      name.value = data.first_name + ' ' + data.last_name;
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
