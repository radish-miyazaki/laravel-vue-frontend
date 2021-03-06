<template>
  <div>
    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
      <div class="btn-toolbar mb-2 mb-md-0" v-if="authenticatedUser.canEdit('roles')">
        <router-link to="/roles/create" class="btn btn-sm btn-outline-secondary">Add</router-link>
      </div>
    </div>

    <h2>Roles</h2>
    <div class="table-responsive">
      <table class="table table-striped table-sm">
        <thead>
        <tr>
          <th>#</th>
          <th>Name</th>
          <td>Action</td>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(role, i) in roles" :key="i">
          <td>{{ role.id }}</td>
          <td>{{ role.name }}</td>
          <td>
            <div class="btn-group mr-2" v-if="authenticatedUser.canEdit('roles')">
              <router-link :to="`/roles/${role.id}/edit`" class="btn btn-sm btn-outline-secondary">Edit</router-link>
              <a href="javascript:void(0)" class="btn btn-sm btn-outline-secondary" @click="del(role.id)">Delete</a>
            </div>
          </td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script lang="ts">
import axios from 'axios';
import { ref, onMounted, computed } from 'vue';
import { Entity } from "@/interfaces/entity";
import { useStore } from "vuex";

export default {
  name: "Roles",

  setup() {
    const roles = ref([]);
    const store = useStore();

    const authenticatedUser = computed(() => store.state.User.user);

    onMounted(async () => {
      const response = await axios.get('roles');

      roles.value = response.data.data;
    });

    const del = async (id: number) => {
      if(confirm('Are you sure you want to delete this record?')) {
        await axios.delete(`roles/${id}`);

        roles.value = roles.value.filter((e: Entity) => e.id !== id);
      }
    };

    return {
      roles,
      del,
      authenticatedUser
    }
  }
}
</script>
