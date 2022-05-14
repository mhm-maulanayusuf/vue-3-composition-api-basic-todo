<template>
  <form @submit.prevent="handleSubmit">
    <div class="row mt-3">
      <div class="col">
        <input type="text" class="form-control" v-model="todo" />
      </div>
      <div class="col-auto">
        <button class="btn btn-success" type="submit">Add</button>
      </div>
    </div>
  </form>
</template>

<script>
import { ref } from "vue";
export default {
  setup(_, { emit }) {
    const todo = ref("");

    function handleSubmit() {
      emit("onSubmit", todo.value);
      todo.value = "";
    }

    return { todo, handleSubmit };
  },
};
</script>
