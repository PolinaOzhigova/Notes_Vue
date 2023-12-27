<template>
  <div class="container mt-5">
    <h2 class="mb-4">Управление заметками</h2>

    <!-- Форма для создания и редактирования заметок -->
    <div class="mb-4">
      <label for="title" class="form-label">Заголовок:</label>
      <input type="text" id="title" v-model="currentNote.title" class="form-control" />

      <label for="content" class="form-label mt-2">Содержимое:</label>
      <textarea id="content" v-model="currentNote.content" class="form-control"></textarea>

      <label class="form-label mt-2">Занятость:</label>
      <div class="mb-2">
        <div class="form-check form-check-inline">
          <input type="checkbox" id="home" v-model="currentNote.categories.home" class="form-check-input" />
          <label for="home" class="form-check-label">Долго</label>
        </div>

        <div class="form-check form-check-inline">
          <input type="checkbox" id="work" v-model="currentNote.categories.work" class="form-check-input" />
          <label for="work" class="form-check-label">Средне</label>
        </div>

        <div class="form-check form-check-inline">
          <input type="checkbox" id="personal" v-model="currentNote.categories.personal" class="form-check-input" />
          <label for="personal" class="form-check-label">Быстро</label>
        </div>
      </div>

      <label for="category" class="form-label">Категория:</label>
      <select id="category" v-model="currentNote.category" class="form-select">
        <option value="home">Дом</option>
        <option value="work">Работа</option>
        <option value="personal">Личное</option>
      </select>

      <label class="form-label mt-2">Приоритет:</label>
      <div class="mb-2">
        <div class="form-check form-check-inline">
          <input type="radio" id="high" value="high" v-model="currentNote.priority" class="form-check-input" />
          <label for="high" class="form-check-label">Высокий</label>
        </div>

        <div class="form-check form-check-inline">
          <input type="radio" id="medium" value="medium" v-model="currentNote.priority" class="form-check-input" />
          <label for="medium" class="form-check-label">Средний</label>
        </div>

        <div class="form-check form-check-inline">
          <input type="radio" id="low" value="low" v-model="currentNote.priority" class="form-check-input" />
          <label for="low" class="form-check-label">Низкий</label>
        </div>
      </div>

      <button @click="saveNote" class="btn btn-primary">Сохранить</button>
    </div>

    <!-- Отображение сохраненных заметок -->
    <div v-if="savedNotes.length">
    <h3 class="mt-4">Сохраненные заметки:</h3>
    <ul class="list-group">
      <li v-for="(note, index) in savedNotes" :key="index" class="list-group-item">
        <strong>{{ note.title }}</strong><br>
        {{ note.content }}<br>
        Занятость: {{ note.categories.home ? 'Долго ' : '' }}{{ note.categories.work ? 'Средне ' : '' }}{{ note.categories.personal ? 'Быстро' : '' }}<br>
        Категория: {{ getCategoryText(note.category) }}<br>
        Приоритет: {{ getPriorityText(note.priority) }}
      </li>
    </ul>
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      currentNote: {
        title: '',
        content: '',
        categories: {
          home: false,
          work: false,
          personal: false,
        },
        category: 'home',
        priority: 'medium',
      },
      savedNotes: [],
    };
  },
  methods: {
    saveNote() {
      if (this.currentNote.title || this.currentNote.content) {
        // Create a new note object with the correct structure
        const newNote = {
          title: this.currentNote.title,
          content: this.currentNote.content,
          categories: { ...this.currentNote.categories },
          category: this.currentNote.category,
          priority: this.currentNote.priority,
        };

        // Push the new note into the savedNotes array
        this.savedNotes.push(newNote);

        // Save the updated array to sessionStorage
        sessionStorage.setItem('notes', JSON.stringify(this.savedNotes));

        // Reset the currentNote object
        this.currentNote = {
          title: '',
          content: '',
          categories: { home: false, work: false, personal: false },
          category: 'home',
          priority: 'medium',
        };
      }
    },

    getCategoryText(category) {
      switch (category) {
        case 'home':
          return 'Дом';
        case 'work':
          return 'Работа';
        case 'personal':
          return 'Личное';
        default:
          return '';
      }
    },

    getPriorityText(priority) {
      switch (priority) {
        case 'high':
          return 'Высокий';
        case 'medium':
          return 'Средний';
        case 'low':
          return 'Низкий';
        default:
          return '';
      }
    },
  },
};
</script>