<template lang="text">
  <div class="pt-2 pb-3 mb-3 border-bottom">
    <router-link to="/roles/create" class="btn btn-sm btn-outline-secondary"
      >Add</router-link
    >
  </div>
  <div class="table-responsive">
    <table class="table table-striped table-sm">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Name</th>
          <th scope="col">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(role, i) in roles" :key="role.id">
          <td>{{ i + 1 }}</td>
          <td>{{ role.name }}</td>
          <td>
            <div class="btn-group mr-2">
              <router-link
                :to="`/roles/${role.id}/edit`"
                class="btn btn-sm btn-outline-info"
                >Edit</router-link
              >
              <a
                href="#"
                class="btn btn-sm btn-outline-secondary"
                @click.prevent="del(role.id)"
                >Delete</a
              >
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { Role } from '@/models/role';
export default {
  name: 'Roles',
  setup() {
    const roles = ref([]);

    onMounted(async () => {
      const { data } = await axios.get('roles');
      roles.value = data;
    });

    const del = async (id: number) => {
      if (confirm('Are you sure?')) {
        await axios.delete(`/roles/${id}`);

        roles.value = roles.value.filter((r: Role) => r.id !== id);
      }
    };

    return {
      roles,
      del,
    };
  },
};
</script>
<style lang=""></style>
