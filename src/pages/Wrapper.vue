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
export default {
  components: {
    NavBar,
    MenuBar,
  },
};
</script>
