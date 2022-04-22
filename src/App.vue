<template>
  <!-- <a-button @click="toggleVisible">选择位置</a-button>
  <MapDialog v-model:visible="visible" /> -->
  <!-- <VirturalList /> --> 
  <VirturalTree />
</template>

<script setup lang="ts">
  // import { ref } from "vue";
  // import MapDialog from "./components/MapDialog.vue"
  // import VirturalList from "./components/VirturalScrollList.vue";
  import VirturalTree from "./components/VirturalScrollTree.vue";
  // const visible = ref(false);
  // const toggleVisible = () => {
  //   visible.value = !visible.value;
  // }

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}
</style>

