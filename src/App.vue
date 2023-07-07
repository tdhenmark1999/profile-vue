<template>
  <div id="app">
    <ProfileForm />
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import ProfileForm from './components/ProfileForm.vue';

export default defineComponent({
  name: 'App',
  components: {
    ProfileForm
  }
});
</script>
