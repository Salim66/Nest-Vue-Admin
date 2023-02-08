<template lang="text">
  <div class="row d-flex justify-content-center mt-5">
    <div class="col-md-8">
      <form @submit.prevent="submit">
        <h1 class="h3 mb-3 fw-normal">Create product</h1>

        <div class="mb-3">
          <label>Title</label>
          <input
            v-model="data.title"
            class="form-control"
            placeholder="Title"
          />
        </div>
        <div class="mb-3">
          <label>Description</label>
          <input
            v-model="data.description"
            class="form-control"
            placeholder="Description"
          />
        </div>
        <div class="mb-3">
          <label>Image</label>
          <input
            v-model="data.image"
            class="form-control"
            placeholder="Image"
          />
        </div>
        <div class="mb-3">
          <label>Price</label>
          <input
            v-model="data.price"
            type="number"
            class="form-control"
            placeholder="Price"
          />
        </div>

        <button class="btn btn-lg btn-primary" type="submit">Save</button>
      </form>
    </div>
  </div>
</template>
<script lang="ts">
import axios from 'axios';
import { reactive } from 'vue';
import { useRouter } from 'vue-router';

export default {
  name: 'Product Create',
  setup() {
    const data = reactive({
      title: '',
      description: '',
      image: '',
      price: '',
    });

    const router = useRouter();

    const submit = async () => {
      await axios.post('products', data);

      await router.push('/products');
    };

    return {
      data,
      submit,
    };
  },
};
</script>
