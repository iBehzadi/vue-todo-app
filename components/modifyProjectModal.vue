<template>
  <div class="top-0 z-10 fixed w-screen h-screen bg-black/50">
    <div class="w-1/2 my-24 mx-auto p-5 rounded-xl bg-white ">
      <form class="flex flex-col ">
        <label>Title</label>
        <input v-model="prjTitle" @keyup="() => { empty = false }" type="text" required>
        <div class="h-3">
          <span class="text-red-500" v-if="empty">Please write some text </span>
        </div>
        <label class="mt-2">Details</label>
        <textarea v-model="prjDetails" class="border border-blue-400"></textarea>
      </form>
      <div class="flex justify-evenly my-5">
        <button @click="close" class="btn">cancel</button>
        <button @click="modifyProject" class="btn">OK</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">

const emit = defineEmits(["close", "ok"]);
const URL: string = 'http://localhost:3002/projects';
const prjTitle = ref<string>('');
const prjDetails = ref<string>('');
const empty = ref<boolean>(false);
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
    //id is true => PATCH data 
    updateProject();
  } else {
    //id is undefined => POST data 
    createProject();
  }
}

function updateProject() {
  if (prjTitle.value?.trim().length) {
    fetch(URL + `/${props.id}`, {
      method: 'PATCH',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ title: prjTitle.value, details: prjDetails.value })
    })
      .then(() => emit('ok'))
      .catch(err => err.message)
  } else {
    empty.value = true;
  }
}

function createProject() {
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

  } else {
    empty.value = true;
  }
}

function close() {
  prjTitle.value = '';
  prjDetails.value = '';
  emit('close');
}
</script>

<style></style>
