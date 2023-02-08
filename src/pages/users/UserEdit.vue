<template lang="">
  <div class="row d-flex justify-content-center mt-5">
    <div class="col-md-6">
      <form @submit.prevent="submit">
        <h1 class="h3 mb-3 fw-normal">Edit user</h1>

        <div class="mb-3">
          <label for="first_name">First Name</label>
          <input
            v-model="data.first_name"
            class="form-control"
            id="first_name"
            placeholder="First Name"
          />
        </div>

        <div class="mb-3">
          <label for="last_name">Last Name</label>
          <input
            v-model="data.last_name"
            class="form-control"
            id="last_name"
            placeholder="Last Name"
          />
        </div>

        <div class="mb-3">
          <label for="email">Email</label>
          <input
            v-model="data.email"
            type="email"
            id="email"
            class="form-control"
            placeholder="Email"
          />
        </div>

        <div class="mb-3">
          <label>Select Role</label>
          <select v-model="data.role_id" class="form-control">
            <option v-for="role of roles" :key="role.id" :value="role.id">
              {{ role.name }}
            </option>
          </select>
        </div>

        <button class="btn btn-lg btn-primary" type="submit">Create</button>
      </form>
    </div>
  </div>
</template>
<script lang="ts">
import { onMounted, reactive, ref } from 'vue';
import axios from 'axios';
import { useRoute, useRouter } from 'vue-router';

export default {
  name: 'User Create',
  setup() {
    const data = reactive({
      first_name: '',
      last_name: '',
      email: '',
      role_id: '',
    });
    const roles = ref([]);
    // this for redirect page
    const router = useRouter();
    // this for get params
    const route = useRoute();

    onMounted(async () => {
      const roleResponse = await axios.get('roles');
      roles.value = roleResponse.data;

      const response = await axios.get(`users/${route.params.id}`);
      data.first_name = response.data.first_name;
      data.last_name = response.data.last_name;
      data.email = response.data.email;
      data.role_id = response.data.role.id;
    });

    const submit = async () => {
      await axios.put(`users/${route.params.id}`, data);

      await router.push('/users');
    };

    return {
      data,
      roles,
      submit,
    };
  },
};
</script>
<style lang=""></style>
