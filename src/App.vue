<template>
  <div>
    <h1 class="text-center display-4 mb-4 mt-4">{{title}}</h1>
    <hr />
    <app-input
      class="text-center"
      v-on:getUserInput="addToList($event)"
      v-on:deleteAll="clearAll()"
    ></app-input>
    <app-list
      class
      v-bind:list="list"
      v-on:updateUI="updateUI($event)"
      v-on:deleteItem="removeFromList($event)"
      v-on:saveEdit="saveEdit($event)"
    ></app-list>
  </div>
</template>

<script>
import InputField from "./components/InputField.vue";
import List from "./components/List.vue";
export default {
  components: {
    "app-input": InputField,
    "app-list": List
  },
  methods: {
    addToList: function(message) {
      this.list.push({
        id: ++this.totalItems,
        message: message,
        checked: false
      });
      this.updateLocalStorage();
    },
    removeFromList: function(id) {
      this.list = this.list.filter(item => item.id != id);
      this.totalItems = 0;
      this.list.forEach(item => (item.id = ++this.totalItems));
      this.updateLocalStorage();
    },
    updateUI: function(item) {
      this.list.forEach(elem => {
        if (elem.id == item) {
          elem.checked = !elem.checked;
        }
      });
      this.updateLocalStorage();
    },
    clearAll: function() {
      this.totalItems = 0;
      this.list = [];
      this.updateLocalStorage();
    },
    saveEdit(obj) {
      console.log(obj);
      this.list.forEach(elem => {
        if (elem.id === obj.id) {
          elem.message = obj.updatedMsg;
        }
      });
      this.updateLocalStorage();
    },
    updateLocalStorage() {
      let tempList = [];
      for (let i = 0; i < this.list.length; i++) {
        let element = this.list[i];
        tempList.push({
          id: element.id,
          checked: element.checked,
          message: element.message
        });
      }
      localStorage.setItem("todolist", JSON.stringify(tempList));
    }
  },
  created() {
    const oldStorageItems = JSON.parse(localStorage.getItem("todolist"));
    console.log(oldStorageItems);
    this.list = oldStorageItems || [];
    // this.totalItems = oldStorageItems.length || 0;

    this.totalItems = oldStorageItems ? oldStorageItems.length : 0;
  },
  data() {
    return {
      title: "WorkToDo",
      totalItems: 0,
      list: []
    };
  }
};
</script>
