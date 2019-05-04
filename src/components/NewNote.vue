<template>
  <div class="new-note">
    <label>Title</label>
    <input v-model="note.title" type="text">
    <label>Description</label>
    <textarea v-model="note.descr"></textarea>
    <div class="container">
        <input type="radio" v-model="note.importance" id="standard" value="standard">
        <label class="importance" for="standard">Standard</label>

        <input type="radio" v-model="note.importance" id="importantly" value="importantly">
        <label class="importance" for="importantly">Importantly</label>

        <input type="radio" v-model="note.importance" id="essential" value="essential">
        <label class="importance" for="essential">Essential</label>
    </div>
    <button class="btn btnPrimary" @click="addNote">New note</button>
  </div>
</template>

<script>
export default {
  props: {
    note: {
      type: Object,
      required: true
    }
  },
  methods: {
    addNote () {
      this.$emit('addNote', this.note)
    }
  }
}
</script>

<style lang="scss">
.new-note {
  text-align: center;
  & > .btn {
    margin: 20px 0;
  }
  & input[type=radio] {
    display: none;
    &:checked + label {
      color: #fff;
    }
    &:not(:checked) + label {
      background: transparent;
    }
  }
  .importance {
    display: inline-block;
    width: 130px;
    margin: 40px 15px 20px;
    padding: 5px;
    border-radius: 40px;
    cursor: pointer;
    transition: all .25s cubic-bezier(.02, .01, .47, 1);
  }
  label[for="standard"] {
    box-shadow: inset 0 0 0 1px #7b7b7b;
    color: #7b7b7b;
    background-color: #7b7b7b;
  }
  label[for="importantly"] {
    box-shadow: inset 0 0 0 1px #ff9a02;
    color: #ff9a02;
    background-color: #ff9a02;
  }
  label[for="essential"] {
    box-shadow: inset 0 0 0 1px #ba3838;
    color: #ba3838;
    background-color: #ba3838;
  }
}
</style>
