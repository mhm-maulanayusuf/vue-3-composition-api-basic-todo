<template>
  <div class="mt-3" v-if="empty">There is no todo...</div>
  <div v-else>
    <div class="list-group mt-3">
      <div
        class="list-group-item list-group-item-action d-flex justify-content-between align-items-center"
        v-for="todo in todos"
        :key="todo"
      >
        <span :class="todo.isDone && 'done'">{{ todo.activity }}</span>
        <div>
          <button
            v-if="!todo.isDone"
            class="btn btn-info btn-sm me-2"
            @click="handleDone(todo)"
          >
            Done
          </button>
          <button
            v-else
            class="btn btn-warning btn-sm me-2"
            @click="handleNotYet(todo)"
          >
            Not Yet
          </button>
          <button class="btn btn-danger btn-sm" @click="handleRemove(todo)">
            X
          </button>
        </div>
      </div>
    </div>
    <div class="mt-3">Total todo: {{ total }}</div>
  </div>
</template>

<script>
import { computed, toRefs, watch } from "vue";

export default {
  props: {
    todos: {
      type: Array,
      default: [],
    },
  },
  setup(props, { emit }) {
    const { todos } = toRefs(props);
    const total = computed(() => todos.value.length);
    const empty = computed(() => total.value <= 0);

    function handleDone(todo) {
      emit("onDone", todo);
    }

    function handleNotYet(todo) {
      emit("onNotYet", todo);
    }

    function handleRemove(todo) {
      emit("onRemove", todo);
    }

    return {
      total,
      empty,
      handleDone,
      handleNotYet,
      handleRemove,
    };
  },
};
</script>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
