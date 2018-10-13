<template>
  <div>
    Name : <input type="text" v-model="form.name"><br>
    Phone : <input type="text" v-model="form.phone"><br>
    <button v-if="!isEdit" @click="add">Save</button>
    <button v-if="isEdit" @click="saveEdit">Edit</button>
    <br>
    <table border="1" width="100%">
      <tr>
        <td>Id</td>
        <td>Name</td>
        <td>Phone</td>
        <td>...</td>
      </tr>
      <tr v-for="(item, k) in items" v-bind:key="k">
        <td>{{item.id}}</td>
        <td>{{item.name}}</td>
        <td>{{item.phone}}</td>
        <td>
          <button @click="initEdit(item)">Edit</button>
          <button @click="del(item.id)">Delete</button>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
  import axios from 'axios'
export default {
  name: 'HelloWorld',
  data () {
    return {
      items: [],
      form: {},
      isEdit: false
    }
  },
  mounted() {
    this.get()
  },
  methods: {
    baseHTTP() {
      return axios.create({
        baseURL: "http://localhost:8020",
        headers: {
          'Content-Type': 'application/json'
        }
      });
    },
    get() {
      this.form = {}
      this.baseHTTP().get("/api/v1/phonebook").then(res => {
        this.items = res.data.data
      })
    },
    add() {
      this.baseHTTP().post("/api/v1/phonebook", this.form).then(res => {
        this.get()
      })
    },
    del(id) {
      if (confirm("Yakin akan menghapus ?")) {
        this.baseHTTP().delete("/api/v1/phonebook/" + id).then(res => {
          this.get()
        })
      }
    },
    initEdit(i) {
      this.form = i
      this.isEdit = true
    },
    saveEdit() {
      this.baseHTTP().put("/api/v1/phonebook/" + this.form.id, this.form).then(res => {
        this.get()
      })
    }
  }
}
</script>

<style scoped>
</style>
