<template>
  <div>
    <div class="card-container">
      <div v-for="user in users" :key="user.id" class="card">
        <img :src="user.avatar" alt="User Avatar" class="avatar" />
        <p>{{ user.first_name }} {{ user.last_name }}</p>
      </div>
    </div>
    <div v-if="pageCount > 1" class="pagination">
      <button v-for="page in pageCount" :key="page" @click="fetchUsers(page)">
        {{ page }}
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      users: [],
      currentPage: 1,
      pageCount: 0,
    };
  },
  created() {
    this.fetchUsers(this.currentPage);
  },
  methods: {
    async fetchUsers(page) {
      try {
        const response = await axios.get(
          `https://reqres.in/api/users?page=${page}`
        );
        this.users = response.data.data;
        this.pageCount = response.data.total_pages;
        this.currentPage = page;
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style scoped>
.card-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  grid-gap: 1rem;
}

.card {
  padding: 1rem;
  background-color: #f2f2f2;
  color: #333;
  font-size: 1.2rem;
  font-weight: 600;
  border-radius: 4px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  margin-bottom: 0.5rem;
}

.pagination {
  margin: 1rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
}

.pagination button {
  margin-right: 0.5rem;
  font-size: 1.2rem;
}
</style>
