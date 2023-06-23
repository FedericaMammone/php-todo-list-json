
<template>
  <div id="app">
    <div class="tasks-container">
      <ul>
        <li class="task" v-for="(task, i) in tasks" :key="i" :class="task.done === true ? 'fatto' : ''"
          @click="invertDone(task)">
          {{ task.text }}

          <i class="fa-solid fa-xmark" @click.stop="deleteTask(i)">
          </i>
        </li>
      </ul>
    </div>

    <!-- aggiunta task -->
    <form @submit.prevent="onSubmit">

      <div class="new-task">
        <label for="task"> </label>
        <input type="text" name="task" placeholder="Nuova task" v-model="newTask.text">
        <button>Aggiungi</button>

        <!-- output errore in pagina -->
        <!-- <div class="error" v-if="error">
    <small style="color:red">Devi inserire un task di almeno 5 caratteri</small>
  </div> -->
      </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {

    return {
      error: false,
      tasks: [],

      newTask: {
        text: "",

      }

    };
  },
  methods: {

    onSubmit() {

      const url = 'http://localhost/Esercizio3-22Giugno/php-todo-list-json/PHP/newTask.php';
      const data = this.newTask;
      const headers = {
        headers: { 'Content-Type': 'multipart/form-data' }
      };


      axios.post(url, data, headers)
        .then(res => {

          const data = res.data;
          this.tasks = data;

          this.newTask.text = "";
        })
    },
    deleteTask(index) {

      const url = 'http://localhost/Esercizio3-22Giugno/php-todo-list-json/PHP/deleteTask.php';
      const data = { "index": index };
      const headers = {
        headers: { 'Content-Type': 'multipart/form-data' }
      };

      axios.post(url, data, headers)
        .then(res => {

          const data = res.data;
          this.tasks = data;
        });
    },
    invertDone(index) {

      if (index.done === true) {
        index.done = false;
      } else {
        index.done = true;

      }
    },
  },
  mounted() {
    axios.post("http://localhost/Esercizio3-22Giugno/php-todo-list-json/PHP/tasks.php")
      .then(res => {
        const data = res.data;
        this.tasks = data;
      })
  }
}

</script>


<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.container {
  margin: 20px auto;
  width: 70%;
  height: 100vh;
}

.new-task {
  text-align: center;
}

.new-task input {
  padding: 10px 20px;
  border-radius: 10px;
  border-width: 1px;
  min-width: 300px;
  margin-right: 10px;
}

.new-task button {
  padding: 10px 20px;
  border-radius: 10px;
  border-width: 1px;
  cursor: pointer;
}

.tasks-container {
  margin: 40px;
  border-radius: 10px;
  background-color: #F5F7F7;
  padding: 30px;
}

.task {
  border-bottom: 1px solid #DADEE1;
  padding: 10px;
  cursor: pointer;
}

.task i {
  float: right;
}

.fatto {
  text-decoration: line-through;
}

.fa-xmark {
  color: red;
  padding: 3px;
  font-size: 25px;
}
</style>
