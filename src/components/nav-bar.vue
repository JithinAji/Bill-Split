<template>
  <div class="flex flex-col md:flex-row justify-between px-12 py-4 bg-grey-lightest">
    <h1 @click="age++" class="text-center text-2xl text-grey-darkest p-3">{{ msg }}</h1>
    <button class="border-4 border-grey-lighter rounded-lg p-3 hover:bg-green-lightest md:hover:bg-grey-light">{{ btn }}</button>
  </div>
</template>

<script>
export default {
  name: 'navbar',
  props: {
    msg: String,
    btn: String,
  },
  data() {
    return{
    }
  },
}
</script>

