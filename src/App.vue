<template>
  <div>
    <Simple
      :image="theimage"
      :columns="columns"
      :rows="rows"
      :canvasHeight="100"
    />
  </div>
</template>

<script>
import { ref } from "vue";
import Simple from "@/components/Simple.vue";

export default {
  components: {
    Simple,
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