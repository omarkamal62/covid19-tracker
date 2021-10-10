<template>
  <div class="text-center">
    <h2 class="text-3xl font-bold">{{ text }}</h2>
    <div class="text-2xl mt-4 mb-10">
      {{ timeStamp }}
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  props: ["text", "dataDate"],
  computed: {
    timeStamp() {
      return moment(this.dataDate).format("MMMM Do YYYY, h:mm:ss a");
    },
  },
};
</script>
