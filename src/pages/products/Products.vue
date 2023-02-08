<template>
  <div class="pt-2 pb-3 mb-3 border-bottom">
    <router-link to="/products/create" class="btn btn-sm btn-outline-secondary"
      >Add</router-link
    >
  </div>
  <div class="table-responsive">
    <table class="table table-striped table-sm">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Image</th>
          <th scope="col">Title</th>
          <th scope="col">Description</th>
          <th scope="col">Price</th>
          <th scope="col">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(product, i) in products" :key="product.id">
          <td>{{ i + 1 }}</td>
          <td><img :src="product.image" alt="" /></td>
          <td>{{ product.title }}</td>
          <td>{{ product.description }}</td>
          <td>{{ product.price }}</td>
          <td>
            <div class="btn-group mr-2">
              <router-link
                :to="`/products/${product.id}/edit`"
                class="btn btn-sm btn-outline-info"
                >Edit</router-link
              >
              <a
                href="#"
                class="btn btn-sm btn-outline-secondary"
                @click.prevent="del(product.id)"
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
import { Product } from '@/models/product';
export default {
  name: 'Product',
  setup() {
    const products = ref([]);
    const page = ref(1);
    const lastPage = ref(0);

    const load = async () => {
      const { data } = await axios.get(`products?page=${page.value}`);
      products.value = data.data;
      lastPage.value = data.meta.last_page;
    };

    // when pagination page is change automaticaly run the load match and jump to the next page
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
        await axios.delete(`products/${id}`);

        products.value = products.value.filter((p: Product) => p.id !== id);
      }
    };

    return {
      products,
      prev,
      next,
      del,
    };
  },
};
</script>
