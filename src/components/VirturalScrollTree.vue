<template>
  <div class="tree-scroller">
    <a-tree
      checkable
      selectable
      v-model:expandedKeys="expandedKeys"
    >
    
      <!-- slots渲染 -->
      <!-- <template #title="row">
        {{ row.title }}
      </template> -->
      
      <!-- TreeNode渲染 -->
      <!-- <template v-for="item in treeData" :key="item.key">
        <a-tree-node :title="item.title">
          <template v-for="subitem in item.children" :key="subitem.key">
            <a-tree-node :title="subitem.title">
              <template v-for="thirditem in subitem.children" :key="thirditem.key">
                <a-tree-node :title="thirditem.title"></a-tree-node>
              </template>  
            </a-tree-node>
          </template>
        </a-tree-node>
      </template> -->

      <template #title="row">
        {{ row }}
        <!-- <RecycleScroller
          class="scroller"
          :items="flatTreeToList(row)"
          :item-size="30"
          v-if="treeData.length"
        >
          <template #default="{item}">
            {{ item.title }}
          </template>
        </RecycleScroller> -->
      </template>

    </a-tree>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, useSlots } from 'vue';
import 'vue-virtual-scroller/dist/vue-virtual-scroller.css';
import { RecycleScroller } from 'vue-virtual-scroller';
const slots = useSlots();
console.log(slots);
const expandedKeys = ref<string[]>(['0-0-0', '0-0-1']);

const treeData = [
  {
    title: '0-0',
    key: '0-0',
    children: [
      {
        title: '0-0-0',
        key: '0-0-0',
        children: [
          { title: '0-0-0-0', key: '0-0-0-0' },
          { title: '0-0-0-1', key: '0-0-0-1' },
          { title: '0-0-0-2', key: '0-0-0-2' },
          { title: '0-0-0-3', key: '0-0-0-3' },
          { title: '0-0-0-4', key: '0-0-0-4' },
          { title: '0-0-0-5', key: '0-0-0-5' },
          { title: '0-0-0-6', key: '0-0-0-6' },
          { title: '0-0-0-7', key: '0-0-0-7' },
          { title: '0-0-0-8', key: '0-0-0-8' },
          { title: '0-0-0-9', key: '0-0-0-9' },
        ],
      },
      {
        title: '0-0-1',
        key: '0-0-1',
        children: [
          { title: '0-0-1-0', key: '0-0-1-0' },
          { title: '0-0-1-1', key: '0-0-1-1' },
          { title: '0-0-1-2', key: '0-0-1-2' },
          { title: '0-0-1-3', key: '0-0-1-3' },
          { title: '0-0-1-4', key: '0-0-1-4' },
          { title: '0-0-1-5', key: '0-0-1-5' },
          { title: '0-0-1-6', key: '0-0-1-6' },
          { title: '0-0-1-7', key: '0-0-1-7' },
          { title: '0-0-1-8', key: '0-0-1-8' },
          { title: '0-0-1-9', key: '0-0-1-9' },
        ],
      },
    ],
  },
];


// 拍平树形结构并添加相应字段
const resList = ref([]);
const flatTreeToList = (data) => {
  function travelTree(data) {
    for(let i in data) {
      resList.value.push({
        title: data.title,
        key: data.key,
        level: data.level,
        isExpand: data.isExpand,
        isShow: data.isShow,
      })
      if(i === 'children') {
        travelTree(data.children)
      }
    }
  }
  travelTree(data);
  return resList;
};

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
