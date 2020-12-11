<template>
  <nav class="col-md-2 d-none d-md-block bg-light sidebar">
    <div class="sidebar-sticky">
      <ul class="nav flex-column">
        <li class="nav-item">
          <router-link to="/" class="nav-link active">
            Dashboard
          </router-link>
          <router-link to="/users" class="nav-link active">
            Users
          </router-link>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {
name: "Menu"
}
</script>