<template>
  <div class="container">
    <div class="profile">
      <h1>Postingan Pengguna</h1>
      <div class="select-container">
        <select id="users" class="select-post" v-model="selectedUserId" @change="loadUserPosts">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
      </div>
      <div class="user-posts">
        <div v-if="loading">Loading...</div>
        <div v-else>
          <table class="post-table">
            <thead>
              <tr>
                <th>Title</th>
                <th>Body</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="post in userPosts" :key="post.id">
                <td>{{ post.title }}</td>
                <td>{{ post.body }}</td>
                <td width="150">
                  <button @click="editPost(post.id)" class="edit-btn">Edit</button>
                  <button @click="deletePost(post.id)" class="delete-btn">Delete</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [],
      selectedUserId: null,
      userPosts: [],
      loading: false
    };
  },
  mounted() {
    this.fetchUsers();
  },
  methods: {
    fetchUsers() {
      fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())
        .then(data => {
          this.users = data;
        })
        .catch(error => {
          console.error('Error fetching users:', error);
        });
    },
    loadUserPosts() {
      if (!this.selectedUserId) return;
      this.loading = true;
      fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUserId}`)
        .then(response => response.json())
        .then(data => {
          this.userPosts = data;
          this.loading = false;
        })
        .catch(error => {
          console.error('Error fetching user posts:', error);
          this.loading = false;
        });
    },
    deletePost(postId) {
      this.userPosts = this.userPosts.filter(post => post.id !== postId);
    },
    editPost(postId) {
      const postToEdit = this.userPosts.find(post => post.id === postId);
      const newTitle = prompt('Edit Title:', postToEdit.title);
      const newBody = prompt('Edit Body:', postToEdit.body);
      
      if (newTitle !== null && newBody !== null) {
        postToEdit.title = newTitle;
        postToEdit.body = newBody;
      }
    }
  }
};
</script>

<style>
.container {
  display: flex;
  justify-content: center;
}

.profile {
  background-color: #73ede1;
  padding: 20px;
  border-radius: 10px;
  margin-top: 30px;
  margin-bottom: 30px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  max-width: 900px;
  width: 900px;
}

.profile h1 {
  color: #2f6baa;
  text-align: center;
  font-size: 36px;
  font-weight: bold;
}

.select-container {
  display: flex;
  justify-content: center;
  margin-bottom: 25px;
  margin-top: -15px;
}

.select-post {
  padding: 5px 10px;
  border-radius: 5px;
  border: 1px solid #4d6aff;
}

.user-posts {
  margin-top: 20px;
  margin: 0 auto;
}

.post-table {
  width: 100%;
  border-collapse: collapse;
  color: #2f6baa;
}

.post-table th, .post-table td {
  padding: 10px;
  border: 1px solid #4d6aff;
}

.post-table th {
  background-color: #4d6aff;
  color: white;
}

.edit-btn {
  background-color: #ffc107;
  color: white;
  border: none;
  padding: 5px 10px;
  margin-right: 5px;
  border-radius: 5px;
  cursor: pointer;
}

.delete-btn {
  background-color: #dc3545;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
}

.edit-btn:hover {
  background-color: #e0a800;
}

.delete-btn:hover {
  background-color: #c82333;
}
</style>
