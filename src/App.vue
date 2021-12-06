<template>
  <div class="container">
    <div class="row">
      <div class="col-md-8 offset-md-2 text-center">
        <h3 class="mt-5">Todo List || Vuejs</h3>
        <hr />
        <div class="row">
          <div
            class="col-md-6 offset-md-3 d-flex justify-content-between align-items-center"
          >
            <input type="text" v-model="todoText" />
            <button class="btn btn-primary"  @click="addToDo()">Ekle</button>
          </div>
        </div>
        <hr />
        <div class="todo-container">
          <Todo @deleteToDo="deleteToDo($event)" v-for="todo in todoList" :key="todo.id" :todo="todo" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Todo from "./components/Todo.vue";
import axios from "axios";
export default {
  components: {
    Todo,
  },
  data() {
    return {
      todoText: "",
      todoList: [],
    };
  },
  methods: {
    addToDo() {
      axios
        .post("https://vuejs-da4f7-default-rtdb.firebaseio.com/todoList.json", {
          text: this.todoText,
        })
        .then((response) => {
          this.todoList.push({
            id:response.data.name,
            text:this.todoText
          })
          this.todoText="";
        })
        .catch((error) => {
          console.log(error);
        });
    },
    deleteToDo(todoId){
      axios.delete("https://vuejs-da4f7-default-rtdb.firebaseio.com/todoList/"+todoId+".json")
      .then(response=>{
        let index=this.todoList.findIndex((i)=>{
          return i.id==todoId;
        });
        this.todoList.splice(index,1)
        console.log(index);
        console.log(response)})
      .catch(err=>console.log(err));
    }
  },
  
  created() {
    axios
      .get("https://vuejs-da4f7-default-rtdb.firebaseio.com/todoList.json")
      .then((response) => {
        for (let key in response.data) {
          let todo = {
            text: response.data[key].text,
            id:key
          };
          this.todoList.push(todo);
        }
      });
  },
};
</script>

<style></style>
