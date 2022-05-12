<template>
  <div id="app">
    <input type="number" v-model="limit" />
    <!-- added prop to avoid using $parent to make the component more reusable -->
    <Numbers :limit="limit" />
  </div>
</template>

<script>
import Numbers from "@/components/Numbers";

export default {
  name: "App",
  components: {
    Numbers
  },
  data() {
    return {
      limit: 100
    };
  }
};
</script>

<style scoped></style>
