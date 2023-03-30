<template>
  <div>
    <canvasImage
      :image="theimage"
      :columns="columns"
      :rows="rows"
      :canvasHeight="100"
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