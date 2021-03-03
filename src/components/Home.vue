<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
      </v-col>

      <v-col class="mb-4">
        <MultiPicker />
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import MultiPicker from "@/components/MultiPicker";
  export default {
    name: 'Home',
    components: {MultiPicker}
  }
</script>
<style></style>
