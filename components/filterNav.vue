<template>
  <div>
    <nav class="filter-nav flex bg-white">
      <button :class="{active: current == 'all'}" @click="updateFilter('all')">View All</button>
      <button :class="{active: current == 'completed'}" @click="updateFilter('completed')">Completed</button>
      <button :class="{active: current == 'ongoing'}" @click="updateFilter('ongoing')">Ongoing</button>
    </nav>
  </div>
</template>

<script lang="ts" setup>
const emit = defineEmits(["filterChange"]);
const current = ref<string>('all')
function updateFilter(by: string) {
  emit('filterChange', by);
  current.value = by;
}
</script>

<style>
nav button {
  @apply p-2 rounded hover:bg-blue-400 transition-all
}
nav .active {
  @apply bg-blue-400
}
</style>