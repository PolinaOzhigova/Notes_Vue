<template>
  <div id="app">
    <notes-manager />
  </div>
</template>

<script>
import NotesManager from './components/NotesManager.vue';

export default {
  components: {
    NotesManager,
  },
  methods: {
    clearData() {
      // Очистка данных в sessionStorage
      sessionStorage.removeItem('notes');
      location.reload();
    },
  },
};
</script>