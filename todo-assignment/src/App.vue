<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todo = ref([]);
const name = ref("");

const input_content = ref("");
//const input_category = ref(null);

const todo_asc = computed(() =>
  todo.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

const addTodo = () => {
  if (input_content.value.trim() === "") {
    return;
  }

  todo.value.push({
    content: input_content.value,
    done: false,
    createdAt: new Date().getTime(),
  });
  //input_content.value = "";
};

const removeTodo = (todoToRemove) => {
  todo.value = todo.value.filter((t) => t !== todoToRemove);
};
watch(
  todo,
  (newVal) => {
    localStorage.setItem("todo", JSON.stringify(newVal));
  },
  { deep: true }
);

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todo.value = JSON.parse(localStorage.getItem("todo")) || [];
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Hii, Welcome
        <input
          type="text"
          placeholder="Input Your Name"
          v-model="name"
          id="name"
        />
      </h2>
    </section>

    <section class="todo-section">
      <h3>This is your Todo App</h3>

      <form @submit.prevent="addTodo" id="input-form">
        <h3>What do you have to do today?</h3>
        <input
          type="text"
          placeholder="Example: Join a call @ 5pm "
          v-model="input_content"
          id="todo-App"
          name="todo-App"
        />

        <input type="Submit" value="Add Todo" />
      </form>
    </section>

    <section class="todolist">
      <h3>YOUR TODO'S</h3>
      <div class="list" id="todos">
        <div
          v-for="todo in todo_asc"
          :class="`todo-item ${todo.done && `done`}`"
        >
          <label>
            <input type="checkbox" name="tickbox" v-model="todo.done" />
          </label>
          <div class="todo-listing">
            <input type="text" class="text" v-model="todo.content" />
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Remove</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<style scoped></style>
