<template>
  <nav class="navbar navbar-dark fixed-top bg-dark flex-md-nowrap p-0 shadow">
    <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="#">Company name</a>
    <ul class="navbar-nav px-3">
      <li class="nav-item text-nowrap">
        <a class="nav-link" href="javascript:void(0)" @click="logout">Sign out</a>
      </li>
    </ul>
  </nav>
</template>

<script>
import { useRouter } from 'vue-router';

export default {
  name: "Nav",

  setup() {
    const router = useRouter();

    const logout = () => {
      localStorage.clear();
      router.push('/login');
    }
    return {
      logout
    }
  }
}
</script>

<style scoped>

</style>