<template>
  <div class="wrapper">
    <div class="note">
      <div class="inputBlock">
        <input type="text" id="titleInput" v-model="title" autocomplete="off" />
      </div>
      <div class="textBlock">
        <textarea
          name="textarea"
          id="textInput"
          cols="50"
          rows="10"
          v-model="text"
        ></textarea>
      </div>
      <div class="buttonsBlock">
        <button class="save" @click="saveEditNote">Save</button>
        <button class="close" @click="closeEditForm">Close</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["editNote"],
  emits: ["closeEditForm", "saveEditNote"],

  data() {
    return {
      title: this.editNote.title,
      text: this.editNote.text,
    };
  },

  watch: {
    editNote: {
      immediate: true,
      handler(newValue) {
        this.title = newValue.title;
        this.text = newValue.text;
      },
    },
  },

  methods: {
    saveEditNote() {
      this.$emit("saveEditNote", {
        id: this.editNote.id,
        title: this.title,
        text: this.text,
        updated: new Date().toLocaleString(),
      });
    },

    closeEditForm() {
      this.$emit("closeEditForm");
      this.editTitle = "";
      this.editText = "";
    },
  },
};
</script>

<style lang="scss" scoped>
.wrapper {
  position: absolute;
  inset: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  backdrop-filter: blur(10px);

  .note {
    width: 600px;
    height: 300px;
    border-radius: 20px;
    box-shadow: 0px 1px 10px 5px rgba(0, 0, 0, 0.5);

    .inputBlock {
      display: flex;
      justify-content: center;
      align-items: center;

      #titleInput {
        margin: 10px auto;
        width: 180px;
        background: transparent;
        border: none;

        color: white;
        font-family: Montserrat;
        font-size: 26px;
        font-weight: 300;
        letter-spacing: 2px;

        &::placeholder {
          color: rgba(210, 210, 210, 0.5);
        }

        &:focus {
          outline: none;
        }
      }
    }

    .textBlock {
      display: flex;
      justify-content: center;
      align-items: center;

      #textInput {
        box-sizing: border-box;
        margin: 10px 20px;
        padding: 10px;
        min-width: 500px;
        min-height: 160px;
        width: 500px;
        max-width: 500px;
        max-height: 150px;
        border: none;
        background: transparent;
        font-family: Montserrat;
        font-weight: 300;
        font-size: 18px;
        color: white;
        box-shadow: 0px 0px 5px 5px rgba(0, 0, 0, 0.1);
        transition: box-shadow 500ms ease;
        letter-spacing: 2px;

        &::placeholder {
          color: rgba(210, 210, 210, 0.5);
        }

        &:focus {
          outline: none;
          box-shadow: 0px 0px 5px 5px rgba(0, 0, 0, 0.2);
        }
      }
    }

    .buttonsBlock {
      display: flex;
      justify-content: center;
      gap: 10px;
      margin: 10px;

      .save {
        background: rgb(70, 170, 70, 0.8);
      }

      .close {
        background: rgb(170, 70, 70, 0.8);
      }

      button {
        width: 100px;
        background: transparent;
        color: white;
        border: none;
        border: 1px solid rgb(160, 160, 160);
        border-radius: 10px;
        font-family: Montserrat;
        transition: color 500ms ease, border-color 500ms ease;

        &:hover {
          color: white;
          border: 1px solid rgb(255, 255, 255);
        }
      }
    }
  }
}

@media (max-width: 768px) {
  .wrapper {
    .note {
      position: relative;
      top: 50px;
      width: 100vw;
      height: 100vh;
      border-radius: 0px;

      #textInput {
        min-width: 200px !important;
        max-width: 300px;
      }
    }
  }
}
</style>
