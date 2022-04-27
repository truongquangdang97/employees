<template>
  <div class="hello">
    <div>
      <h1>Lọc theo nhân sự theo  tên</h1>
      <input type="text" placeholder="Nhập title" v-model="searchObj.name" >
      <button @click="searchNew()">Search</button>
    </div>
    <h1>Danh sách nhân sự </h1>
    <table class="table" style="margin: auto">
      <thead>
        <tr>
          <th>Id</th>
          <th>Name</th>
          <th>Wage</th>
       
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in data" :key="index">
          <th>{{ item.id }}</th>
          <th>{{ item.name }}</th>
          <th>{{ item.wage }}</th>
          <th>
            <button @click="deleteNew(item.id)">delete</button>
            <button @click="editNew(item.id)">edit</button>
          </th>
        </tr>
      </tbody>
    </table>
    <div>
      <h1>Thêm Nhân sự </h1>
      <button v-if="obj.id > 0" @click="updateNew(obj.id)">Update</button>
      <button v-else  @click="addNew()">Add</button>
      <label for="">name</label>
      <input type="text" v-model="obj.name" />

      <label for="">Wage</label>
      <input type="number" v-model="obj.wage" />

    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      data: [],
      obj: {
        name: "",
        wage: "",
      },
      action: false,
      searchObj:{
        name:'',
      }
    };
  },
  created() {
    this.all();
  },
  methods: {
    searchNew(){
      axios
        .get("http://localhost:8080/api/v1/employees?name="+this.searchObj.name)
        .then((response) => (this.data = response.data));
    },

    all() {
      axios
        .get("http://localhost:8080/api/v1/employees")
        .then((response) => (this.data = response.data));
    },
    addNew() {
      axios
        .post("http://localhost:8080/api/v1/employees", this.obj)
        .then((response) => {
          console.log(response);
          this.all();
          this.obj = {};
        });
    },
    deleteNew(id) {
      console.log(id);
      axios
        .delete("http://localhost:8080/api/v1/employees/" + id)
        .then((response) => {
          console.log(response);
          this.all();
          this.obj = {};
        });
    },
    editNew(id) {
      axios.get("http://localhost:8080/api/v1/employees/" + id).then((response) => {

        this.obj = response.data;
        console.log(this.obj);
      });
    },
    updateNew(id) {
      axios
        .put("http://localhost:8080/api/v1/employees/" + id, this.obj)
        .then((response) => {
          console.log(response);
          this.all();
          this.obj = {};
        });
      this.obj = [];
    },
  },
};
</script>


<style scoped>
.hello{
  text-align: center;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
