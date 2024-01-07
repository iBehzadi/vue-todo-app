<template>
  <div>
    <div class="mt-24 text-center">
      <h1 class=" p-2 my-4 bg-red-400">home page </h1>
    </div>
    <section class="bg-gray-200">
      <div class="section-center ">
        <div class="max-w-[32rem] mx-auto">
          <div class="btns flex justify-start">
            <button @click="addProject">Add Project</button>
          </div>
          <div class="border-b-2 border-blue-300"></div>
          <div v-if="projects?.length">
            <div v-for="project in projects" :key="project.id">
              <SingleProject @edit="editHandle" @delete="handleDelete" @complete="completeHandle" :project="project" />
            </div>
          </div>
          <div v-else>
            <p>no task exist ...</p>
          </div>
        </div>
      </div>
    </section>
    <modifyProjectModal v-if="showModal" :id="editingIdProject" @ok="completeTask" @close="closeModal" />
  </div>
</template>

<script lang="ts" setup>
function closeModal() {
  showModal.value = !showModal.value;
  editingIdProject.value = 0;
}
function completeTask() {
  showModal.value = !showModal.value;
  editingIdProject.value = 0;
  getJsonData();
}
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
const editingIdProject = ref<number>();
const URL: string = 'http://localhost:3002/projects';

async function getJsonData() {
  const result = await httpGet<IProject[]>(URL)
  projects.value = result
}

onMounted(() => {
  //get data from server
  // (async () => {
  //   const result = await httpGet<IProject[]>(URL)
  //   projects.value = result
  // })()

  getJsonData();
  // fetch('http://localhost:3002/projects')
  //   .then(res => res.json())
  //   .then(data => projects.value = data)
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
  showModal.value = !showModal.value
  editingIdProject.value = id
}
function addProject(): void {
  showModal.value = true
}
async function httpGet<T>(url: string): Promise<T> {
  return fetch(url)
    .then(Response => Response.json());
}

</script>

<style></style>