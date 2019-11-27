<template>
  <div>
    <input
      class="form-control form-control-lg w-75 ml-auto mr-auto text-center"
      v-model="message"
      type="text"
      v-on:keyup.enter="getUserInput"
      placeholder="Type here anything..."
      autofocus
    />&nbsp;
    <br />
    <button v-on:click="getUserInput()" class="btn btn-success">Add to list</button>
    <button v-on:click="deleteAll()" class="btn btn-danger">Delete All</button>
  </div>
</template>

<script>
export default {
  methods: {
    getUserInput: function() {
      if (this.message.length > 0) {
        this.$emit("getUserInput", this.message);
        this.message = "";
      }
    },
    deleteAll: function() {
      this.$emit("deleteAll");
    }
  },
  data() {
    return {
      message: ""
    };
  }
};
</script>

