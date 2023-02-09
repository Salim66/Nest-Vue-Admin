<template>
  <nav
    id="sidebarMenu"
    class="col-md-3 col-lg-2 d-md-block bg-light sidebar collapse"
  >
    <div class="position-sticky pt-3">
      <ul class="nav flex-column">
        <li class="nav-item">
          <router-link to="" active-class="active" class="nav-link">
            <span data-feather="home"></span>
            Dashboard
          </router-link>
          <router-link to="/users" active-class="active" class="nav-link">
            <span data-feather="home"></span>
            Users
          </router-link>
          <router-link to="/roles" active-class="active" class="nav-link">
            <span data-feather="home"></span>
            Roles
          </router-link>
          <router-link to="/products" active-class="active" class="nav-link">
            <span data-feather="home"></span>
            Products
          </router-link>
          <router-link to="/orders" active-class="active" class="nav-link">
            <span data-feather="home"></span>
            Orders
          </router-link>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script lang="ts">
export default {};
</script>
