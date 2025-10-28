<template>
  <div class="p-4 max-w-md mx-auto">
    <h1 class="text-4xl font-bold mb-2">Hello Note</h1>

    <!-- Title input -->
    <input
        type="text"
        placeholder="Title"
        v-model="note.title"
        class="w-full border p-2 mb-2"
    />

    <!-- Content textarea -->
    <textarea
        placeholder="Content"
        v-model="note.content"
        rows="5"
        class="w-full border p-2 mb-2"
    ></textarea>
    <input
      type="text"
      placeholder="Tags"
      v-model="note.tags"
      class="w-full border p-2 mb-2"
      />


    <!-- Save button -->
    <button
        class="bg-sky-500 text-black px-6 py-2 rounded"
        @click="saveNote"
    >
      Save Locally
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, onMounted } from 'vue';

// Define a TypeScript type for your note
type Note = {
  title: string;
  content: string;
  tags: string;
};

export default defineComponent({
  name: 'HelloNote',
  setup() {
    const note = reactive<Note>({ title: '', content: '', tags: '' });

    // Load note from localStorage on mount
    onMounted(() => {
      const stored = localStorage.getItem('myNote');
      if (stored) {
        try {
          const parsed: Note = JSON.parse(stored);
          note.title = parsed.title;
          note.content = parsed.content;
          note.tags = parsed.tags;
        } catch (e) {
          console.error('Failed to parse stored note', e);
        }
      }
    });

    // Save note to localStorage
    const saveNote = () => {
      localStorage.setItem('myNote', JSON.stringify(note));
      alert('Note saved locally!');
    };

    return {
      note,
      saveNote
    };
  }
});
</script>

<style scoped>
/* Optional extra styling if you like */
</style>
