<template lang="text">
  <div class="row d-flex justify-content-center mt-5">
    <div class="col-md-8">
      <form @submit.prevent="submit">
        <h1 class="h3 mb-3 fw-normal">Edit product</h1>

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
          <div class="input-group">
            <input
              v-model="data.image"
              class="form-control"
              placeholder="Image"
            />
            <ImageUpload @uploaded="data.image = $event"/>
          </div>
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

        <button class="btn btn-lg btn-primary" type="submit">Update</button>
      </form>
    </div>
  </div>
</template>
<script lang="ts">
import axios from 'axios';
import { reactive, onMounted } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import ImageUpload from '@/components/ImageUpload.vue';

export default {
  name: 'Product Create',
  components: {
    ImageUpload,
  },
  setup() {
    const data = reactive({
      title: '',
      description: '',
      image: '',
      price: '',
    });

    const router = useRouter();
    const route = useRoute();

    onMounted(async () => {
      const response = await axios.get(`products/${route.params.id}`);

      data.title = response.data.title;
      data.description = response.data.description;
      data.image = response.data.image;
      data.price = response.data.price;
    });

    const submit = async () => {
      await axios.put(`products/${route.params.id}`, data);

      await router.push('/products');
    };

    return {
      data,
      submit,
    };
  },
};
</script>
