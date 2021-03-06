<template>
  <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
    <div class="btn-toolbar mb-2 mb-md-0">
      <a href="javascript:void(0)" class="btn btn-sm btn-outline-secondary" @click="exportFile">Export</a>
    </div>
  </div>

  <h2>Orders</h2>
  <div class="table-responsive">
    <table class="table table-striped table-sm">
      <thead>
      <tr>
        <th>#</th>
        <th>Name</th>
        <th>Email</th>
        <th>Total</th>
        <th>Action</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(order, i) in orders" :key="i" >
        <td>{{ order.id }}</td>
        <td>{{ order.first_name }} {{ order.last_name }}</td>
        <td>{{ order.email }}</td>
        <td>{{ order.total }}</td>
        <td>
          <div class="btn-group mr-2">
            <router-link :to="`/orders/${order.id}`" class="btn btn-sm btn-outline-secondary">View</router-link>
          </div>
        </td>
      </tr>
      </tbody>
    </table>
    <Paginator :last-page="lastPage" @page-changed="load($event)"/>
  </div>
</template>

<script>
import { ref, onMounted, computed } from 'vue';
import axios from 'axios';
import Paginator from "@/secure/components/Paginator";
import { useStore } from "vuex";

export default {
  name: "Orders",

  components: {
    Paginator
  },

  setup() {
    const orders = ref([]);
    const lastPage = ref(0);
    const store = useStore();

    const authenticatedUser = computed(() => store.state.User.user);

    const load = async (page = 1) => {
      const response = await axios.get(`orders?page=${page}`);
      orders.value = response.data.data;
      lastPage.value = response.data.meta.last_page;
    }

    const exportFile = async () => {
      const response = await axios.get('export', {responseType: 'blob'});
      const blob = new Blob([response.data], {type: 'text/csv'});
      const downloadUrl = window.URL.createObjectURL(response.data);
      const link = document.createElement('a');
      link.href = downloadUrl;
      link.download = 'orders.csv';
      link.click();
    }

    onMounted(load);

    return {
      orders,
      lastPage,
      load,
      exportFile,
      authenticatedUser
    }
  }
}
</script>