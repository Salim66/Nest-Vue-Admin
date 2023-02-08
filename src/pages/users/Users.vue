<template>
  <div class="table-responsive">
    <table class="table table-striped table-sm">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Name</th>
          <th scope="col">Email</th>
          <th scope="col">Role</th>
          <th scope="col">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(user, i) in users" :key="user.id">
          <td>{{ i + 1 }}</td>
          <td>{{ user.first_name }} {{ user.last_name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.role.name }}</td>
          <td>
            <div class="btn-group mr-2">
              <a
                href="#"
                class="btn btn-sm btn-outline-secondary"
                @click.prevent="del(user.id)"
                >Delete</a
              >
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
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
import { onMounted, ref, watch } from 'vue';
import axios from 'axios';
import { User } from '@/models/user';
export default {
  name: 'Users',
  setup() {
    const users = ref([]);
    const page = ref(1);
    const lastPage = ref(0);

    const load = async () => {
      const { data } = await axios.get(`users?page=${page.value}`);
      users.value = data.data;
      lastPage.value = data.meta.last_page;
    };

    watch(page, load);

    onMounted(load);

    const next = () => {
      if (page.value < lastPage.value) {
        page.value++;
      }
    };

    const prev = () => {
      if (page.value > 1) {
        page.value--;
      }
    };

    const del = async (id: number) => {
      if (confirm('Are you sure?')) {
        await axios.delete(`users/${id}`);

        users.value = users.value.filter((u: User) => u.id !== id);
      }
    };

    return {
      users,
      prev,
      next,
      del,
    };
  },
};
</script>
