<template>
  <PokeDex />
</template>

<script lang="ts">
import { defineComponent } from 'vue';

// Components
import PokeDex from '../components/PokeDex.vue';

export default defineComponent({
  name: 'HomeView',

  components: {
    PokeDex,
  },
});
</script>

