<template>
  <div class="currentNote">
    <div class="noteInfo">
      <div class="title">
        {{ currentNote.title }}
      </div>
      <div class="text" :style="{ fontSize: fontSizeValue }">
        {{ currentNote.text }}
      </div>
      <div class="s">
        <div class="date">
          Created:
          {{ currentNote.created }}
        </div>
        <div class="updated">
          Updated:
          {{ currentNote.updated }}
        </div>
      </div>
    </div>
    <div class="options">
      <button id="edit" @click="editCurrentNote(currentNote.id)">Edit</button>
      <button id="remove" @click="removeCurrentNote(currentNote.id)">
        Delete
      </button>
      <button id="inc" @click="incFont">+</button>
      <button id="dec" @click="decFont">-</button>
      <button id="close" @click="closeCurrentNote">Close</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["currentNote", "editNote"],
  emits: ["closeCurrentNote", "removeCurrentNote", "editCurrentNote"],

  data() {
    return {
      fontSize: 20,
    };
  },

  computed: {
    fontSizeValue() {
      return `${this.fontSize}px`;
    },
  },

  methods: {
    closeCurrentNote() {
      this.$emit("closeCurrentNote");
    },

    removeCurrentNote(id) {
      this.$emit("removeCurrentNote", id);
    },

    editCurrentNote(id) {
      this.$emit("editCurrentNote", id);
    },

    incFont() {
      this.fontSize += 2;
    },

    decFont() {
      this.fontSize -= 2;
    },
  },

  mounted() {},
};
</script>

<style lang="scss" scoped>
.currentNote {
  width: 540px;
  border-radius: 20px;
  max-height: 310px;
  backdrop-filter: blur(5px);
  box-shadow: 0 0 10px 5px rgba(0, 0, 0, 0.5);

  .options {
    margin: 16px;
    padding: 2px;
    display: flex;
    justify-content: center;
    align-content: center;
    box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.1);
    border-radius: 20px;

    #edit {
      position: absolute;
      left: 10px;
      top: 10px;
      background: rgb(70, 170, 70, 0.8);
    }

    #remove {
      position: absolute;
      left: 60px;
      top: 10px;
      background: rgb(170, 70, 70, 0.8);
    }

    #inc {
      position: absolute;
      right: 100px;
      top: 10px;
      background: rgb(70, 110, 70, 0.8);
    }

    #dec {
      position: absolute;
      right: 74px;
      top: 10px;
      background: rgb(200, 70, 70, 0.8);
    }

    #close {
      position: absolute;
      right: 10px;
      top: 10px;
      background: rgb(170, 70, 70, 0.8);
    }

    button {
      width: fit-content;
      margin: 5px;
      background: transparent;
      color: white;
      border: 1px solid rgba(210, 210, 210, 0.5);
      border-radius: 20px;
      transition: color 500ms linear, border-color 500ms ease;
      font-family: Montserrat;
      font-weight: 400;

      &:hover {
        color: white;
        border: 1px solid white;
      }
    }
  }

  .noteInfo {
    color: white;
    font-weight: 400;
    font-size: 30px;
  }

  .title {
    margin: 30px auto 10px;
    text-align: center;
    font-size: 20px;
    font-weight: 300;
  }

  .text {
    padding: 10px;
    margin: 5px 20px;
    height: 100px;
    font-weight: 200;
    box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.1);
    overflow: auto;

    scroll-behavior: auto;

    // Скрываем полосу прокрутки в WebKit-браузерах (Chrome, Safari)
    &::-webkit-scrollbar {
      display: none;
    }

    // Скрываем полосу прокрутки в Firefox
    scrollbar-width: none;

    // Для Internet Explorer и старых версий Edge
    -ms-overflow-style: none;
  }

  .s {
    margin: 20px;
    display: flex;
    justify-content: space-around;
  }

  .date {
    font-size: 14px;
    font-weight: 300;
  }

  .updated {
    font-size: 14px;
    font-weight: 300;
  }
}

@media (max-width: 768px) {
  .currentNote {
    margin: 20px;
    border: 1px solid white;
    height: 200px;

    .text {
      overflow: auto;
      height: 50px;
    }
  }
}
</style>
