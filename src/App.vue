<script setup>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';

  const favWeb = ref("");
  const favWebs = ref([]);

  const displayFavWebs = () => {
    const localFavWebs = localStorage.getItem("favoriteWeb");
    if(null != localFavWebs)
    {
      favWebs.value = JSON.parse(localFavWebs);
    }
  }
  const clearFavWebs = () => {
    favWebs.value = [];
    localStorage.removeItem("favoriteWeb");
  }
  const addFavWeb = () => {
    if("" != favWeb.value)
    {
      if(isValidURL(favWeb.value))
      {
        favWebs.value.push(favWeb.value);
        localStorage.setItem("favoriteWeb", JSON.stringify(favWebs.value));
        favWeb.value = "";
      }
      else
      {
        alert("請輸入正確的網址格式！");
      }
    }
  }
  const deleteFavWeb = (index) => {
    favWebs.value.splice(index, 1);
    localStorage.setItem("favoriteWeb", JSON.stringify(favWebs.value));
  };

  const isValidURL = (str) => {
    try {
      new URL(str);
      return true;
    } catch (_) {
      return false;
    }
  };
</script>

<template>
  <h1 class="text-3xl">收藏網址</h1>
 
  <div class="header-row">
    <h3 class="text-3xl">輸入網址 :</h3>
    <!-- <input @keyup.enter="addFavWeb" v-model.trim="favWeb" type="text" class="input"></input>    -->
     <input @keyup.enter="addFavWeb" v-model.trim="favWeb" type="text" class="input"></input>
  </div>

  <div>
    <button @click= "addFavWeb" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">新增</button>
    <button @click= "displayFavWebs" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">顯示</button>
    <button @click= "clearFavWebs" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">清空全部</button>
  </div>
  
  <li v-for="(favWeb, index) in favWebs" :key="index">
    {{ favWeb }}
    <button @click="deleteFavWeb(index)" class="ml-2 text-red-500">刪除</button>
  </li>
</template>

<style scoped>
  @reference "./style.css";
</style>
