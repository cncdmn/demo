<template>
  <div class="tree-scroller">
    <RecycleScroller
      class="scroller"
      :items="items"
      :item-size="30"
      v-if="items.length"
    >
      <template #default="{ item }">
        <div class="user">
          {{ item.title }}
        </div>
      </template>
    </RecycleScroller>

  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import "vue-virtual-scroller/dist/vue-virtual-scroller.css"
import { RecycleScroller } from 'vue-virtual-scroller';
interface Item {
  title: string;
  id: number;
}
const items = ref<Item[]>([]);
const getData = () => {
  for (let i = 0; i < 100; i++) {
    items.value.push({ title: 'ssadqwesadwqe' ,id : i})
  }
};
onMounted(() => {
  getData();
})
</script>

<style scoped>
.scroller {
  width: 300px;
  height: 300px;
  background-color: rgba(0, 0, 0, 0.1);
}

.user {
  padding: 0 12px;
  display: flex;
  align-items: center;
}
</style>