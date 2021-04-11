<template>
  <navbar msg="Bill Split" btn="Add Event" />
  <div class="flex flex-row flex-wrap justify-between mx-24">
    <eventBox class="w-full h-auto m-6" eventName="Trip" />
  </div>
</template>

<script>
import navbar from "./components/nav-bar.vue";
import eventBox from "./components/event-box.vue";

export default {
  name: "App",
  components: {
    navbar,
    eventBox,
  },
  data() {
    return {
      events: 0,
    };
  },
  methods: {},
};
</script>

