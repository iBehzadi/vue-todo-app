<template>
  <div>
    <div class="mt-24 text-center">
      <h1 class=" p-2 my-4 bg-red-400">home page </h1>
      <NuxtLink to="/about">about page</NuxtLink>
    </div>
    <section class="bg-gray-200">
      <div class="section-center">
        <div v-if="projects?.length">
          <div v-for="project in projects" :key="project.id">
            <SingleProject @delete="handleDelete" :project="project" />
          </div>
        </div>
      </div>
    </section>
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

const projects = ref<IProject[]>();
const URL:string = 'http://localhost:3002/projects';

onMounted(() => {
  //get data from server
  (async () => {
    const result = await httpGet<IProject[]>(URL)
    projects.value = result
  })()

  // fetch('http://localhost:3002/projects')
  //   .then(res => res.json())
  //   .then(data => projects.value = data)
})
function handleDelete(id:number){
  projects.value = projects.value?.filter(item => {return item.id !== id})
}
async function httpGet<T>(url: string): Promise<T> {
  return fetch(url)
    .then(Response => Response.json());
}

</script>

<style></style>