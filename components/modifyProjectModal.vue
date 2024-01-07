<template>
  <div class="top-0 z-10 fixed w-screen h-screen bg-black/50">
    <div class="w-1/2 my-24 mx-auto p-5 rounded-xl bg-white text-center">
      <form class="flex flex-col">
        <label>Title</label>
        <input v-model="prjTitle" type="text" required>
        <label>Details</label>
        <textarea v-model="prjDetails" class="border border-blue-400" required></textarea>
      </form>
      <div class="flex justify-evenly my-5">
        <button @click="close" class="btn">cancel</button>
        <button @click="modifyProject" class="btn">OK</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
function close() {
  prjTitle.value = '';
  prjDetails.value = '';
  emit('close');
}
const emit = defineEmits(["close", "ok"]);
const URL: string = 'http://localhost:3002/projects';
const prjTitle = ref<string>();
const prjDetails = ref<string>();
const props = defineProps<{
  id?: number
}>();

onMounted(() => {
  if (props.id) {
    //id is true => PATCH data // create a function for edit
    (async () => {
      const result: IProject = await (await fetch(URL + `/${props.id}`)).json()
      prjTitle.value = result.title
      prjDetails.value = result.details
    })();
  }
})


function modifyProject(): void {
  if (props.id) {
    //id is true => PATCH data // create a function for edit

    //edit project
    if (prjTitle.value?.trim().length) {
      fetch(URL + `/${props.id}`, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ title: prjTitle.value, details: prjDetails.value })
      })
        .then(() => emit('ok'))
        .catch(err => err.message)
    }

  } else {
    //id is undefined => POST data // create a function for post
    let project;
    if (prjTitle.value?.trim().length) {
      project = {
        title: prjTitle.value,
        details: prjDetails.value,
        complete: false
      };
      fetch(URL, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(project)
      })
        .then(() => emit('ok'))
        .catch(err => console.log(err.message))

    }
  }


}
</script>

<style></style>
