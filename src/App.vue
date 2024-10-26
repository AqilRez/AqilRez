<script setup>
import { onMounted, ref } from "vue";
import PocketBase from "pocketbase";

const pb = new PocketBase('http://127.0.0.1:8090');

const titleVal = ref("");
const data = ref([]);

async function Submit() {

// example create data
const record = {
    title: titleVal.value,
    description: "desc",
};

  await pb.collection('posts').create(record);

  data.value.push(record);
  }

onMounted(async() => {
  const result = await pb.collection('posts').getFullList();

  data.value = result;
})
</script>

<template>
<input class="bg-stone-700" v-model="titleVal">
<button @click="Submit">Add</button>

<ul>
  <li v-for="item in data">
    {{item.title}}
  </li>
</ul>
</template>