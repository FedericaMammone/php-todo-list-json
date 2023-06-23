
<template>
  <form @submit.prevent="onSubmit">
    <div class="new-task">
      <input type="text" placeholder="Nuova task" v-model="newTask" @keyup.enter="aggiungiTask">
      <button @click="aggiungiTask">Aggiungi</button>

      <!-- output errore in pagina -->
      <div class="error" v-if="error">
        <small style="color:red">Devi inserire un task di almeno 5 caratteri</small>
      </div>
    </div>
  </form>
  <div class="tasks-container">
    <ul>
      <li class="task" v-for="(task, index) in tasks" :class="task.done === true ? 'fatto' : ''"
        @click.stop="invertDone(task)">
        {{ task.text }}

        <i class="fa-solid fa-xmark" @click.stop="rimuoviTask"></i>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {

    return {
      tasks: [],

      error: false,
      newTask: '',
      text: '',

    };
  },
  mounted() {
    axios.get("http://localhost/Esercizio3-22Giugno/php-todo-list-json/PHP/")
      .then(res => {
        const data = res.data;
        this.tasks = data;
      });
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
</style>
