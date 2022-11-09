<template>
  <q-page class="row items-center justify-evenly">
    <input-form-component />
  </q-page>
</template>

<script lang="ts">
import InputFormComponent from 'src/components/InputFormComponent.vue';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'IndexPage',
  components: { InputFormComponent },
  setup() {
    return {};
  },
});
</script>
