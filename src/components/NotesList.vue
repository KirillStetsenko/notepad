<template>
  <aside class="aside">
    <header class="header">
      <button class="btn" @click="incSort">a - z</button>
      <button class="btn" @click="decSort">z - a</button>
      <button class="btn" @click="recently">newest</button>
      <button class="btn" @click="old">oldest</button>
    </header>
    <ul>
      <li v-for="note in notes" :key="note.id" @click="setCurrentNote(note.id)">
        {{ note.title }}
        <button class="removeBtn" @click="remove(note.id)">X</button>
      </li>
    </ul>
  </aside>
</template>

<script>
export default {
  props: ["notes"],
  emits: ["setCurrentNote", "remove", "incSort", "decSort", "recently", "old"],

  methods: {
    setCurrentNote(id) {
      this.$emit("setCurrentNote", id);
    },

    remove(id) {
      this.$emit("remove", id);
    },

    incSort() {
      this.$emit("incSort");
    },

    decSort() {
      this.$emit("decSort");
    },

    recently() {
      this.$emit("recently");
    },

    old() {
      this.$emit("old");
    },
  },
};
</script>

<style lang="scss" scoped>
.aside {
  border-radius: 16px;
  backdrop-filter: blur(5px);
  box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.5);
  height: 300px;
  overflow-y: scroll;
  overflow-x: hidden;
  scroll-behavior: auto;

  &::-webkit-scrollbar {
    display: none;
  }

  scrollbar-width: none;

  -ms-overflow-style: none;

  .header {
    width: 300px;
    display: flex;
    justify-content: center;
    gap: 10px;
    margin: 16px;
    padding: 2px;
    border-radius: 20px;
    box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.3);
    background: rgba(210, 201, 210, 0.3);

    .btn {
      width: fit-content;
      margin: 5px;
      background: transparent;
      color: white;
      border: 1px solid rgba(210, 210, 210, 0.5);
      border-radius: 20px;
      transition: color 500ms linear, border-color 500ms ease;
      font-family: Montserrat;
      font-weight: 500;

      &:hover {
        color: white;
        border: 1px solid white;
      }
    }
  }

  ul {
    margin: 20px;
    padding: 0;
    list-style: none;
    border-radius: 20px;

    li {
      display: flex;
      justify-content: end;
      align-content: center;
      gap: 10px;
      height: 30px;
      margin: 20px;
      padding-left: 10px;
      border-radius: 20px;
      box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.1);
      color: rgba(210, 210, 210, 0.5);
      font-size: 20px;
      font-weight: 200;
      line-height: 30px;

      &:hover {
        color: white;
      }

      .removeBtn {
        width: 30px;
        height: 30px;
        text-align: center;
        background: rgb(170, 70, 70, 0.8);
        color: white;
        border: 1px solid rgba(210, 210, 210, 0.5);
        border-radius: 50%;
        font-family: Montserrat;
        transition: color 500ms ease, border-color 500ms ease;

        &:hover {
          color: white;
          border: 1px solid white;
        }
      }
    }
  }
}

@media (max-width: 768px) {
  .aside {
    overflow-y: auto;
    max-height: 250px;
    position: absolute;
    bottom: 30px;
  }
}
</style>
