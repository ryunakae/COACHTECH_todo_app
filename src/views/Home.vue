<template>
  <div class="home">
    <div class="card mt-5 mb-5">
      <div class="card-body form-group">
        <h2 class="card-title">ToDo List</h2>
        <div class="mb-4">
          <input type="text" v-model="newItem" class="col-sm-8">
          <button @click="postItem" class="btn btn-outline-primary btn-sm mt-2 mb-2 ml-5">追加</button>
        </div>
        <div v-for="(data,index) in items" :key="index">
          <div class="mb-2">
            <input type="text" v-model="data.item" class="mb-1 col-sm-6">
            <button @click="putItem(data.id, data.item)" class="btn btn-outline-warning btn-sm ml-5">更新</button>
            <button @click="deleteItem(data.id)" class="btn btn-outline-danger btn-sm ml-2">削除</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import axios from "axios";

export default {
  data() {
    return {
      newItem: "",
      items: []
    }
  },
  methods: {
    async getItems() {
      const toDoItems = await axios.get("http://127.0.0.1:8000/api/todo_item");
      this.items = toDoItems.data.data;
    },
    async postItem() {
      const toDoItem = {item: this.newItem};
      await axios.post("http://127.0.0.1:8000/api/todo_item", toDoItem);
      await this.getItems();
    },
    async putItem(id, item) {
      const toDoItem = {item: item};
      await axios.put("http://127.0.0.1:8000/api/todo_item/" + id, toDoItem);
    },
    async deleteItem(id) {
      await axios.delete("http://127.0.0.1:8000/api/todo_item/" + id);
      await this.getItems();
    }
  },
  created() {
    this.getItems();
  },
  name: 'Home',
  components: {
    // HelloWorld
  }
}
</script>

<style>
  .home {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
  }
  .card {
    width: 70%;
  }
</style>