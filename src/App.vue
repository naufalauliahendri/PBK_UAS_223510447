<template>
  <q-layout view="hHh lpR fFf">
    <q-header class="header">
      <q-toolbar>
        <q-btn-group flat class="rout-nav">
          <router-link to="/">
            <q-btn flat :outline="!isActive('/')" :class="{ active: isActive('/') }" label="Todos" />
          </router-link>
          <router-link to="/post">
            <q-btn flat :outline="!isActive('/post')" :class="{ active: isActive('/post') }" label="Post" />
          </router-link>
          <router-link to="/albums">
            <q-btn flat :outline="!isActive('/albums')" :class="{ active: isActive('/albums') }" label="Albums" />
          </router-link>
        </q-btn-group>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <q-page>
        <router-view />
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import { useRoute } from 'vue-router';

export default {
  setup() {
    const route = useRoute();
    const isActive = (path) => route.path === path;
    return {
      isActive
    };
  }
};
</script>

<style scoped>
.header {
  background-color: #f5f5f5;
  color: #00bfff !important;
  padding: 10px;
  box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.2);
  background-image: url(/src/assets/image/3401742.jpg);
  background-size: cover;
  display: flex;
  justify-content: center; 
  border-bottom: 1px dotted #82e6d5;
}

.rout-nav {
  display: flex;
  justify-content: center;
  width: 100%;
}

.rout-nav button {
  color: #00bfff;
  font-size: 16px;
}

.rout-nav button:hover {
  background-color: transparent;
}

.rout-nav:hover {
  color: #00bfff;
  text-decoration: line-through;
  background-color: transparent;
}

.rout-nav link:active {
  text-decoration: line-through;
}

.rout-nav a q-btn {
  color: #ffc0cb;
  font-size: 16px;
}

.rout-nav q-btn[flat][outline] {
  border-color: #ffc0cb;
  color: #ffc0cb;
}

.active {
  text-decoration: line-through;
}
</style>
