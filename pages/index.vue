<template>
  <div class="mt-24">
    <h1 class="text-center p-2 my-4 bg-red-400">home page </h1>
  </div>
  <section class="bg-gray-200">
    <div class="section-center">
      <div v-if="projects?.length">
        <div v-for="project in projects" :key="project.id">{{ project.title }}</div>
      </div>
    </div>
  </section>
</template>

<script lang="ts" setup>
interface IProject {
  id: number,
  title: string,
  details: string,
  complete: boolean
}

const projects = ref<IProject[]>();

onMounted(() => {
  (async () => {
    const result = await httpGet<IProject[]>('http://localhost:3002/projects')
    projects.value = result
  })()

  // fetch('http://localhost:3002/projects')
  //   .then(res => res.json())
  //   .then(data => projects.value = data)
})

async function httpGet<T>(url: string): Promise<T> {
  return fetch(url)
    .then(Response => Response.json());
}

</script>

<style></style>