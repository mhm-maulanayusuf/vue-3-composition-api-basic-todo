<template>
  <div class="container">
    <h3>Simple Todo App</h3>
    <form-add @onSubmit="add" />
    <list-todo
      :todos="todos"
      @onDone="done"
      @onNotYet="notYet"
      @onRemove="remove"
    />
  </div>
</template>

<script>
import { onMounted, reactive, ref } from "vue";
import FormAdd from "./components/FormAdd.vue";
import ListTodo from "./components/ListTodo.vue";

export default {
  components: { FormAdd, ListTodo },
  setup() {
    const todos = ref([]);

    function saveToLocalStorage() {
      localStorage.setItem("todos", JSON.stringify(todos.value));
    }

    onMounted(() => {
      let todosFromStorage = JSON.parse(localStorage.getItem("todos"));
      todos.value = todosFromStorage ? todosFromStorage : [];
    });

    function add(todo) {
      if (todo != "") {
        todos.value.unshift({
          activity: todo,
          isDone: false,
        });
      }
      saveToLocalStorage();
    }

    function done(todo) {
      todo.isDone = true;
      saveToLocalStorage();
    }

    function notYet(todo) {
      todo.isDone = false;
      saveToLocalStorage();
    }

    function remove(todo) {
      todos.value = todos.value.filter(
        (value) => value.activity != todo.activity
      );
      saveToLocalStorage();
    }

    return {
      todos,
      add,
      done,
      notYet,
      remove,
    };
  },
};
</script>
