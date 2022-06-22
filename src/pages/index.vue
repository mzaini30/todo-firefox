<script setup="" lang="ts">
  import {backup} from 'backup-state'
  import {ref} from 'vue'

  const {stringify, parse} = JSON
  const data = ref([])
  const inputan = ref('')
  const dataBaru = ref('')
  const mauTambah = ref(false)

  function cek(): void {
    if (localStorage.todo){
      data.value = parse(localStorage.todo)
    }
  }
  cek()

  function tambah(): void{
    data.value = [inputan.value, ...data.value]
    inputan.value = ''
    localStorage.todo = stringify(data.value)
  }

  function simpanDataBaru(): void {
    data.value = parse(dataBaru.value)
    localStorage.todo = dataBaru.value
    dataBaru.value = ''
    mauTambah.value = false
  }

  function hapus(x){
    data.value = data.value.filter(y => y != x)
    localStorage.todo = stringify(data.value)
  }
</script>

<template>
  <div class="p-2 flex text-sm pb-1 text-gray-600 gap-2 menu">
    <a @click.prevent='backup(data, "todo.json")' href="/">backup</a>
    <a @click.prevent="mauTambah = true" href="/">restore</a>
    <a href="https://github.com/mzaini30/todo-firefox">github</a>
  </div>

  <form v-if="mauTambah" class="p-2 relative bg-orange-200 block" @submit.prevent="simpanDataBaru" action="">
    <span @click="mauTambah = false" class="absolute top-0 right-2 text-gray-500 cursor-pointer">&times;</span>
    <p class="mb-1">Write data here</p>
    <textarea required v-model="dataBaru" class="block text-sm w-full p-1" name="" id="" cols="30" rows="10"></textarea>
    <input type="submit" class="bg-green-500 rounded text-white px-3 py-1 mt-2 inline-block cursor-pointer" value="Process"/>
  </form>

  <div class="p-2 px-3 py-1 bg-pink-200 overflow-auto max-h-50">
    <div class="text-sm" v-for="x in data">
      {{x}} 
      <span @click='hapus(x)' class="underline cursor-pointer hover:text-pink-500">(done)</span>
    </div>
  </div>

  <form action="" @submit.prevent='tambah' class="p-2">
    <input type="text" v-model='inputan' class="w-full p-1 text-sm border border-blue-500 caret-pink-500 text-pink-700 focus:border-pink-500 focus:outline-none" placeholder='Write, Enter'/>
  </form>
</template>

<style scoped="">
* {
  word-wrap: break-word;
}
.menu a {
  @apply hover:(underline decoration-pink-500 underline-2 text-pink-700)
}
</style>