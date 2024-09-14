<template>
  <div class="app">
    <header class="header">
      <corner-git></corner-git>
      <div class="title">Notepad</div>
      <div class="welcome" v-show="!notes.length">Create your first Note</div>
    </header>
    <main class="main">
      <notes-list
        v-if="notes.length"
        :notes="notes"
        @setCurrentNote="setCurrentNote"
        @remove="remove"
        @incSort="incSortName"
        @decSort="decSortName"
        @recently="recentlyCreated"
        @old="oldCreated"
      ></notes-list>

      <template v-if="currentNote">
        <current-note
          :currentNote="currentNote"
          @editCurrentNote="edit"
          @closeCurrentNote="close"
          @removeCurrentNote="remove"
        ></current-note>
        <edit-note-form
          v-show="editNote"
          :editNote="editNote"
          @closeEditForm="closeEditForm"
          @saveEditNote="saveEditNote"
        ></edit-note-form>
      </template>
      <new-note-form
        class="newNoteForm"
        v-else
        @createNewNote="createNewNote"
      ></new-note-form>
    </main>
    <div class="errors">
      <div class="titleError" v-show="titleError">
        Note title must be beetween 4-16 char
      </div>

      <div class="textError" v-show="textError">
        Note text must be more than 4 char
      </div>
    </div>
    <footer class="footer">2024</footer>
  </div>
</template>

<script>
import NotesList from "./components/NotesList.vue";
import CurrentNote from "./components/CurrentNote.vue";
import EditNoteForm from "./components/EditNoteForm.vue";
import NewNoteForm from "./components/NewNoteForm.vue";
import CornerGit from "./components/CornerGit.vue";

export default {
  components: {
    NotesList,
    NewNoteForm,
    CurrentNote,
    EditNoteForm,
    CornerGit,
  },
  data() {
    return {
      editNote: "",
      currentNote: "",
      notes: [],
      titleError: false,
      textError: false,
      missingTextError: false,
    };
  },

  methods: {
    incSortName() {
      this.notes = this.notes.sort((a, b) => a.title.localeCompare(b.title));
    },

    decSortName() {
      this.notes = this.notes.sort((a, b) => b.title.localeCompare(a.title));
    },

    recentlyCreated() {
      this.notes = this.notes.sort(
        (a, b) => new Date(b.created) - new Date(a.created)
      );
    },

    oldCreated() {
      this.notes = this.notes.sort(
        (a, b) => new Date(a.created) - new Date(b.created)
      );
    },

    setCurrentNote(id) {
      this.currentNote = this.notes.find((note) => note.id === id);
      this.editNote = "";
    },

    createNewNote(newNote) {
      this.titleError = false;
      this.textError = false;
      this.missingTextError = false;

      // Проверяем длину title
      if (newNote.title.length < 4 || newNote.title.length > 40) {
        this.titleError = true;
      }

      if (newNote.text.length < 4) {
        this.textError = true;
      }

      if (!this.textError && this.titleError) {
        this.missingTextError = true;
      }

      if (!this.titleError && !this.textError) {
        this.notes.push(newNote);
        this.setNotes();
      }
    },

    close() {
      this.currentNote = "";
    },

    edit(id) {
      this.editNote = this.notes.find((note) => note.id == id);
    },

    remove(id) {
      this.notes = this.notes.filter((note) => note.id !== id);
      this.close();
      this.setNotes();
    },

    saveEditNote({ id, title, text, updated }) {
      const targetNote = this.notes.find((note) => note.id == id);
      targetNote.title = title;
      targetNote.text = text;
      targetNote.updated = updated;
      this.closeEditForm();
      this.setNotes();
    },

    closeEditForm() {
      this.editNote = "";
    },

    setNotes() {
      localStorage.setItem("notes", JSON.stringify(this.notes));
    },

    getNotes() {
      const notes = JSON.parse(localStorage.getItem("notes"));
      if (Array.isArray(notes)) {
        this.notes = notes;
      } else {
        this.notes = [];
      }
    },
  },

  mounted() {
    this.getNotes();
  },
};
</script>

<style lang="scss" scoped>
.app {
  font-family: Montserrat, sans-serif;
  height: 100vh;

  .title {
    margin-top: 10px;
    text-align: center;
    color: #d7aad7;
    font-size: 60px;
    font-weight: 300;
    letter-spacing: 10px;
  }

  .welcome {
    margin-top: 20px;
    color: orange;
    text-align: center;
    font-size: 20px;
    font-weight: 200;
    letter-spacing: 5px;
  }

  .main {
    display: flex;
    justify-content: center;

    gap: 30px;
    margin: 20px auto;
  }

  .errors {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    font-size: 14px;

    .titleError {
      margin: 10px;
      color: orange;
    }

    .textError {
      color: orange;
    }
  }

  .footer {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translate(-50%);
    color: white;
  }
}

@media (max-width: 768px) {
  .header {
    text-align: center;

    .title {
      font-size: 30px;
    }
  }
}
</style>
