<template>
  <div class="table-responsive">
    <table class="table table-sm">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Name</th>
          <th scope="col">Email</th>
          <th scope="col">Total</th>
          <th scope="col">Action</th>
        </tr>
      </thead>
      <tbody>
        <template v-for="(order, i) in orders" :key="order.id">
          <tr>
            <td>{{ i + 1 }}</td>
            <td>{{ order.name }}</td>
            <td>{{ order.email }}</td>
            <td>{{ order.total }}</td>
            <td>
              <div class="btn-group mr-2">
                <a
                  href="javascript:void(0)"
                  class="btn btn-sm btn-outline-secondary"
                  >View</a
                >
              </div>
            </td>
          </tr>
          <tr>
            <td colspan="5">
              <div>
                <table class="table table-sm">
                  <thead>
                    <tr>
                      <th scope="col">#</th>
                      <th scope="col">Product Title</th>
                      <th scope="col">Quantity</th>
                      <th scope="col">Price</th>
                    </tr>
                  </thead>
                  <tbody v-for="(item, i) in order.order_items" :key="item.id">
                    <tr>
                      <td>{{ i + 1 }}</td>
                      <td>{{ item.product_title }}</td>
                      <td>{{ item.quantity }}</td>
                      <td>{{ item.price }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </td>
          </tr>
        </template>
      </tbody>
    </table>
  </div>
  <Paginator :lastPage="lastPage" @page-changed="load($event)" />
</template>

<script lang="ts">
import { onMounted, ref } from 'vue';
import axios from 'axios';
import Paginator from '@/components/paginator.vue';
export default {
  name: 'Orders',
  components: {
    Paginator,
  },
  setup() {
    const orders = ref([]);
    const lastPage = ref(0);

    const load = async (page = 1) => {
      const { data } = await axios.get(`orders?page=${page}`);
      orders.value = data.data;
      lastPage.value = data.meta.last_page;
    };

    onMounted(load);

    return {
      orders,
      lastPage,
      load,
    };
  },
};
</script>
