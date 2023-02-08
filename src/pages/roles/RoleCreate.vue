<template lang="text">
  <div class="row d-flex justify-content-center mt-5">
    <div class="col-md-10">
      <form @submit.prevent="submit">
        <h1 class="h3 mb-3 fw-normal">Create Role</h1>

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
              <input class="form-check-input" type="checkbox" :value="permission.id" @change="select(permission.id, $event.target.checked)" />
              <label class="form-check-label">{{ permission.name }}</label>
            </div>
          </div>
        </div>

        <button class="btn btn-lg btn-primary" type="submit">Save</button>
      </form>
    </div>
  </div>
</template>
<script>
import { onMounted, reactive, ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
export default {
  name: 'Role Create',
  setup() {
    const formData = reactive({
      name: '',
      permissions: [],
    });
    const permissionList = ref([]);
    const router = useRouter();

    onMounted(async () => {
      const { data } = await axios.get('permissions');
      permissionList.value = data;
    });

    const select = (id, checked) => {
      if (checked) {
        formData.permissions = [...formData.permissions, id];
        return;
      }

      formData.permissions = formData.permissions.filter((p) => p !== id);
    };

    const submit = async () => {
      await axios.post('roles', formData);

      await router.push('/roles');
    };

    return {
      formData,
      permissionList,
      select,
      submit,
    };
  },
};
</script>
