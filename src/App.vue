<script setup>
import { ref, reactive, onUpdated } from "vue";

let id = localStorage.getItem("id") || 1;
const input = ref();
const list = reactive(JSON.parse(localStorage.getItem("list")) || []);

function changeLocalStorage() {
  localStorage.setItem("list", JSON.stringify(list));
  localStorage.setItem("id", id);
}

function addItem() {
  if (input.value.trim() === "") {
    return;
  }

  list.push({
    id: id,
    name: input.value,
    checked: false,
  });

  input.value = "";
  id++;
}

function removeItem(id) {
  for (let index = 0; index < list.length; index++) {
    if (id === list[index].id) {
      list.splice(index, 1);
      break;
    }
  }
}

onUpdated(() => {
  changeLocalStorage();
});
</script>

<template>
  <body @keydown.enter="addItem">
    <div class="container text-bg-dark mt-5 p-4 rounded-4">
      <header class="d-flex gap-4">
        <input type="text" class="form-control" v-model="input" placeholder="Tarefa" />
        <button class="btn btn-light" @click="addItem">Adicionar</button>
      </header>
      <hr class="my-4" />
      <main>
        <section v-if="list.length > 0">
          <div v-for="item in list" :key="item.id" class="d-flex p-3 rounded-2">
            <input type="checkbox" class="me-3" v-model="item.checked" />
            <p class="m-0">{{ item.name }}</p>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              fill="currentColor"
              class="ms-auto bi bi-trash3"
              viewBox="0 0 16 16"
              @click="removeItem(item.id)"
            >
              <path
                d="M6.5 1h3a.5.5 0 0 1 .5.5v1H6v-1a.5.5 0 0 1 .5-.5M11 2.5v-1A1.5 1.5 0 0 0 9.5 0h-3A1.5 1.5 0 0 0 5 1.5v1H1.5a.5.5 0 0 0 0 1h.538l.853 10.66A2 2 0 0 0 4.885 16h6.23a2 2 0 0 0 1.994-1.84l.853-10.66h.538a.5.5 0 0 0 0-1zm1.958 1-.846 10.58a1 1 0 0 1-.997.92h-6.23a1 1 0 0 1-.997-.92L3.042 3.5zm-7.487 1a.5.5 0 0 1 .528.47l.5 8.5a.5.5 0 0 1-.998.06L5 5.03a.5.5 0 0 1 .47-.53Zm5.058 0a.5.5 0 0 1 .47.53l-.5 8.5a.5.5 0 1 1-.998-.06l.5-8.5a.5.5 0 0 1 .528-.47M8 4.5a.5.5 0 0 1 .5.5v8.5a.5.5 0 0 1-1 0V5a.5.5 0 0 1 .5-.5"
              />
            </svg>
          </div>
        </section>
        <section v-else>
          <h4 class="text-center">Lista vazia</h4>
        </section>
      </main>
    </div>
  </body>
</template>

<style scoped>
section > div:nth-child(2n - 1) {
  background-color: #242932;
}

input[type="checkbox"] {
  display: grid;
  grid-template-columns: 1.2em auto;
  cursor: pointer;
}

input:checked + p {
  color: #959495;
  text-decoration: line-through;
}

svg {
  cursor: pointer;
}
</style>
