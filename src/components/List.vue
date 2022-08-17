<template>
  <ul class="task-list">
    <Item v-for="task in tasks" :key="task.id" :task="task" />
  </ul>
</template>

<script>
import Item from "@/components/Item.vue";
export default {
  props: {
    tasks: {
      type: Array,
      required: true,
    },
  },
  components: {
    Item,
  },
};
</script>

<style>
.task-list {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  margin: 0 auto;
  gap: 15px;
}
</style>
