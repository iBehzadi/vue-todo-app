<template>
  <div class="bg-gray-200 h-screen">
    <div class="mt-24 text-center">
      <h1 class=" p-2  bg-red-400">home page </h1>
    </div>
    <section>
      <div class="section-center ">
        <div class=" mx-auto">
          <div class="btns flex items-center">
            <button @click="addProject" class="bg-white mx-2 p-2 rounded hover:bg-blue-400 transition-all">Add
              Project</button>
            <FilterNav @filterChange="current = $event" />
          </div>
          <div class="border-b-2 border-blue-300 my-1"></div>
          <div v-if="projects?.length" class="lg:flex flex-wrap">
            <div v-for="project in filteredProjects" :key="project.id" class="basis-1/2" :class="{}">
              <SingleProject @edit="editHandle" @delete="handleDelete" @complete="completeHandle" :project="project" />
            </div>
          </div>
          <div v-else>
            <p>no task exist ...</p>
          </div>
        </div>
      </div>
    </section>
    <modifyProjectModal v-if="showModal" :id="editProjectId" @ok="completeTask" @close="closeModal" />
  </div>
</template>

<script lang="ts" setup>

declare global {
  interface IProject {
    id: number,
    title: string,
    details: string,
    complete: boolean
  }
}
const showModal = ref<boolean>(false)
const projects = ref<IProject[]>();
const editProjectId = ref<number>();
const URL: string = 'http://localhost:3002/projects';
const current = ref<string>('all');

async function getJsonData() {
  const result = await httpGet<IProject[]>(URL);
  projects.value = result;
}

onMounted(() => {
  getJsonData();

})
const filteredProjects = computed(() => {
  if (current.value === 'completed') {
    return projects.value?.filter(item => item.complete)
  } else if (current.value === 'ongoing') {
    return projects.value?.filter(item => !item.complete)
  } else {
    return projects.value
  }
})
function handleDelete(id: number): void {
  projects.value = projects.value?.filter(item => { return item.id !== id })
}
function completeHandle(id: number): void {
  let p = projects.value?.find(item => { return item.id === id });
  if (p?.complete !== undefined) {
    p.complete = !p.complete
  }
}
function editHandle(id: number) {
  showModal.value = !showModal.value;
  editProjectId.value = id;
}
function closeModal() {
  showModal.value = !showModal.value;
  editProjectId.value = 0;
}
function completeTask() {
  showModal.value = !showModal.value;
  editProjectId.value = 0;
  getJsonData();
}
function addProject(): void {
  showModal.value = true;
}

async function httpGet<T>(url: string): Promise<T> {
  return fetch(url)
    .then(Response => Response.json());
}

</script>

<style></style>