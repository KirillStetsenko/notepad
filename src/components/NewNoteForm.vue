<template>
  <div class="form">
    <div class="titleBlock">
      <input
        type="text"
        id="titleInput"
        v-model="title"
        placeholder="New title..."
        autocomplete="off"
        maxlength="16"
      />
    </div>
    <div class="textBlock">
      <textarea
        name="textarea"
        id="textInput"
        v-model="text"
        placeholder="New text..."
      ></textarea>
    </div>
    <div class="createBlock">
      <button class="createBtn" @click="createNewNote">Create</button>
      <button class="clearBtn" @click="delText">Clear</button>
    </div>
  </div>
</template>

<script>
import { v4 as uuidv4 } from "uuid";
export default {
  emits: ["createNewNote"],

  data() {
    return {
      title: "",
      text: "",
    };
  },

  methods: {
    createNewNote() {
      this.$emit("createNewNote", {
        id: uuidv4(),
        title: this.title,
        text: this.text,
        created: new Date().toLocaleString(),
      });
      this.title = "";
      this.text = "";
    },

    delTitle() {
      this.title = "";
    },

    delText() {
      this.text = "";
      this.title = "";
    },
  },
};
</script>

<style lang="scss" scoped>
.form {
  padding: 10px;
  border-radius: 20px;
  max-height: 300px;
  backdrop-filter: blur(5px);
  box-shadow: 0 0 10px 5px rgba(0, 0, 0, 0.5);

  .titleBlock {
    margin: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  #titleInput {
    margin: 10px;
    width: 200px;
    max-width: fit-content;
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

  .textBlock {
    box-sizing: border-box;
    width: fit-content;
    margin: 0 auto;
    text-align: center;

    #textInput {
      box-sizing: border-box;
      margin: 10px 20px;
      padding: 10px;
      min-width: 300px;
      min-height: 150px;
      width: 500px;
      max-width: 600px;
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

  .createBlock {
    display: flex;
    padding: 10px;
    justify-content: center;
    text-align: center;
    gap: 10px;

    .createBtn {
      width: 100px;
      background: rgb(70, 170, 70, 0.8);
      color: white;
      border: none;
      border: 1px solid rgb(160, 160, 160);
      border-radius: 50px;
      font-family: Montserrat;
      transition: color 500ms ease, border-color 500ms ease;

      &:hover {
        color: white;
        border: 1px solid rgb(255, 255, 255);
      }
    }

    .clearBtn {
      width: 100px;
      text-align: center;
      background: rgb(170, 70, 70, 0.8);
      color: white;
      border: 1px solid rgba(210, 210, 210, 0.5);
      border-radius: 50px;
      font-family: Montserrat;
      transition: color 500ms ease, border-color 500ms ease;

      &:hover {
        color: white;
        border: 1px solid white;
      }
    }
  }
}

@media (max-width: 768px) {
  .form {
    margin: 10px;
    margin-top: 20px;
    padding: 0;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;

    .titleBlock {
      margin: 0;

      #titleInput {
        font-size: 20px;
      }
    }

    .textBlock {
      height: 100px;
      #textInput {
        width: 300px;
        min-height: unset;
        max-height: unset;
        font-size: 16px;
        overflow: hidden;
      }
    }
  }
}
</style>
