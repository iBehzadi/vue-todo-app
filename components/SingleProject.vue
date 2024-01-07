<template>
  <div
    class="project  my-5 mx-auto bg-white py-2 px-5 rounded-md shadow-sm border-l-4 border-solid border-l-[#e90074] "
    :class="{ completed: project.complete }">
    <div class="actions  flex justify-between items-center">
      <h3 class="cursor-pointer" @click="showDetail = !showDetail">{{ project.title }}</h3>
      <div>
        <span @click="emit('edit', props.project.id)">
          <Icon name="uil:pen" size="1.5em" />
        </span>
        <span @click="deleteProject">
          <Icon name="uil:trash" size="1.5em" />
        </span>
        <span @click="changeComplete">
          <Icon name="uil:check" size="1.5em" />
        </span>
      </div>
    </div>
    <div v-show="showDetail" class="details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script lang="ts" setup>
const props = defineProps<{ project: IProject }>();
const URL: string = 'http://localhost:3002/projects/' + props.project.id;
const showDetail = ref<boolean>(false);

const emit = defineEmits(['delete', 'complete', 'edit']);
function deleteProject():void {
  fetch(URL, { method: 'DELETE' })
    .then(() => { emit('delete', props.project.id) })
    .catch((err) => { console.log(err.message) })
}
function changeComplete():void {
  fetch(URL, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ complete: !props.project.complete })
  })
    .then(() => { emit('complete', props.project.id) })
    .catch((err) => { console.log(err.message) })
}
</script>

<style>
.actions span {
  @apply text-gray-400 hover:text-black cursor-pointer
}

.completed {
  @apply border-l-[blue]
}
</style>