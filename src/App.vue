<template>
  <div>
    <canvasImage
      :image="theimage"
      :columns="columns"
      :rows="rows"
    />
  </div>
</template>

<script>
import { ref } from "vue";
import canvasImage from "@/components/canvasImage.vue";

export default {
  components: {
    canvasImage,
  },
  setup() {
    const theimage = ref("/logo.png");
    const columns = ref(8);
    const rows = ref(8);

    return {
      theimage,
      columns, 
      rows
    };
  },
};
</script>

<style>
html,
body,
#app {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
}
</style>