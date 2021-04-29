<template>
  <section v-for="user in users" :key="user.id">
    <h3>{{ user.email }}</h3>
  </section>
  <div class="mt-16 px-16 mx-auto xl:max-w-3xl">
    <div class="mt-10">
      <button
        class="bg-indigo-500 text-gray-100 p-4 w-full rounded-full tracking-wide font-semibold font-display focus:outline-none focus:shadow-outline hover:bg-indigo-600 shadow-lg"
        @click="handleLogout()"
      >
        Logout
      </button>
    </div>
  </div>
</template>

<script lang='ts'>
import { defineComponent } from "vue";
import { logout } from "@/api/auth";
import router from "@/router";
import axios from "axios";

export default defineComponent({
  name: "Home",
  setup() {
    const handleLogout = () => {
      logout().then(() => {
        router.push("/");
      });
    };
    return {
      handleLogout,
    };
  },

  data() {
    return {
      users: [],
      user_id: this.$route.params.user_id,
    };
  },
  mounted() {
    let uid = localStorage.getItem("uid");

    axios
      .get(`http://localhost:3000/api/v1/users/${this.user_id}.json`)
      .then((response) => (this.users = response.data));
  },
});
</script>