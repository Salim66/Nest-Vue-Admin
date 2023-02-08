<template lang="text">
  <div class="row d-flex justify-content-center mt-5">
    <div class="col-md-10">
      <form @submit.prevent="submit">
        <h1 class="h3 mb-3 fw-normal">Edit Role</h1>

        <div class="mb-3 mt-3 row">
          <label for="col-sm-2 col-form-level">Name</label>
          <input
            v-model="formData.name"
            class="form-control"
            placeholder="Name"
          />
        </div>

        <div class="mb-3 row">
          <label for="col-sm-2 col-form-level">Permissions</label>
          <div class="col-sm-10">
            <div class="form-check form-check-inline col-3" v-for="permission in permissionList" :key="permission.id"> 
              <input class="form-check-input" type="checkbox" :value="permission.id"
                :checked="checked(permission.id)"
               @change="select(permission.id, $event.target.checked)" />
              <label class="form-check-label">{{ permission.name }}</label>
            </div>
          </div>
        </div>

        <button class="btn btn-lg btn-primary" type="submit">Update</button>
      </form>
    </div>
  </div>
</template>
<script lang="ts">
import { onMounted, reactive, ref } from 'vue';
import axios from 'axios';
import { useRoute, useRouter } from 'vue-router';
import { Permission } from '@/models/permission';
export default {
  name: 'Role Create',
  setup() {
    const formData = reactive({
      name: '',
      permissions: [] as number[],
    });
    const permissionList = ref([]);
    const router = useRouter();
    const route = useRoute();

    onMounted(async () => {
      const { data } = await axios.get('permissions');
      permissionList.value = data;

      const response = await axios.get(`roles/${route.params.id}`);
      formData.name = response.data.name;
      formData.permissions = await response.data.permissions.map(
        (p: Permission) => p.id
      );
    });

    const select = (id: number, checked: boolean) => {
      if (checked) {
        formData.permissions = [...formData.permissions, id];
        return;
      }

      // uncheck id remove
      formData.permissions = formData.permissions.filter((p) => p !== id);
    };

    const submit = async () => {
      await axios.put(`roles/${route.params.id}`, formData);

      await router.push('/roles');
    };

    const checked = (id: number) => formData.permissions.some((p) => p === id);

    return {
      formData,
      permissionList,
      select,
      submit,
      checked,
    };
  },
};
</script>
