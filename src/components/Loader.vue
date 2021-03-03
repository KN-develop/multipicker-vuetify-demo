<template>
  <v-overlay>
    <v-card
        color="primary"
        dark
    >
      <v-card-text>
        Грузим. Пожалуйста подождите.
        <v-progress-linear
            indeterminate
            color="white"
            class="mb-0"
        ></v-progress-linear>
      </v-card-text>
    </v-card>
  </v-overlay>
</template>
<script>
export default {
  name: 'Loader',
}
</script>
