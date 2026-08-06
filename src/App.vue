<script setup lang="ts">
import { ref, computed, onMounted, watch } from "vue";

interface tasksInterface {
  id: number;
  title: string;
  completed: boolean;
}

interface TodoAPI {
  id: number;
  title: string;
  completed: boolean;
}

const tasks = ref<tasksInterface[]>([]);
const text = ref<string>("");
const filter = ref<string>("Todas");
const carregando = ref<boolean>(true);

const filteredTasks = computed(() => {
  if (filter.value == "Nao-concluidas") {
    return tasks.value.filter((T) => T.completed == false);
  } else if (filter.value == "Concluidas") {
    return tasks.value.filter((T) => T.completed == true);
  }

  return tasks.value;
});

function newTask() {
  tasks.value.push({
    id: Date.now(),
    title: text.value,
    completed: false,
  });

  text.value = "";
}

function deleteTask(id: number) {
  if (id) {
    tasks.value.splice(
      tasks.value.findIndex((T) => T.id == id),
      1,
    );
  }
}

function markDone(id: number) {
  if (id) {
    const task = tasks.value[tasks.value.findIndex((T) => T.id == id)]!;

    task.completed = !task.completed;
  }
}

const pendentes = computed(() => {
  return tasks.value.filter((t) => !t.completed).length;
});

watch(
  tasks,
  (newTasks, _oldTasks) => {
    const backup = JSON.stringify(newTasks);

    localStorage.setItem("backup", backup);
  },
  { deep: true },
);

onMounted(async () => {
  const backupverify = localStorage.getItem("backup");

  if (backupverify) {
    const backupdata = JSON.parse(backupverify);
    tasks.value.push(...backupdata);
  } else {
    const res = await fetch(
      "https://jsonplaceholder.typicode.com/todos?_limit=5",
      { method: "GET" },
    );
    const data: TodoAPI[] = await res.json();

    const newtasks: tasksInterface[] = data.map((a) => ({
      id: a.id,
      title: a.title,
      completed: a.completed,
    }));

    tasks.value.push(...newtasks);
    console.log(data);
  }
  carregando.value = false;
});
</script>

<template>
  <p v-if="carregando">Carregando...</p>

  <p>Tarefas a concluir: {{ pendentes }}</p>

  <label for="filter"></label>
  <select name="filter" id="filter" v-model="filter">
    <option value="Todas">Todas</option>
    <option value="Nao-concluidas">Não concluidas</option>
    <option value="Concluidas">Concluidas</option>
  </select>

  <br /><br />

  <input
    type="text"
    placeholder="Nome da task"
    v-model="text"
    @keyup.enter="newTask"
  />
  <button type="button" @click="newTask">Adicionar</button>

  <ul>
    <li v-for="t in filteredTasks" :key="t.id">
      <p :class="t.completed ? 'done' : ''">{{ t.title }}</p>
      <button type="button" @click="deleteTask(t.id)">Deletar</button>
      <button type="button" @click="markDone(t.id)">
        {{ !t.completed ? "Concluir" : "Desconcluir" }}
      </button>
    </li>
  </ul>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
