<template>
  <div class="fullList">
    <ul class="mt-lg-5">
      <li v-for="listItem in list">
        <div class="row">
          <div class="col-9" style="word-break: break-word;">
            <!-- <span>{{listItem.id}}.&nbsp;</span> -->
            <input
              v-on:click="completedItem(listItem)"
              type="checkbox"
              v-bind:checked="listItem.checked"
              v-bind:id="listItem.id"
            />
            <label
              v-bind:for="listItem.id"
              v-bind:class="{checked: listItem.checked}"
            >{{listItem.message}}</label>
          </div>
          <!-- display only on small sized devices -->
          <div class="col-3 d-block d-sm-block d-md-none text-right">
            <div class="dropdown dropleft">
              <button
                class="btn btn-outline-info dropdown-toggle"
                type="button"
                id="dropdownMenuButton"
                data-toggle="dropdown"
                aria-haspopup="true"
                aria-expanded="false"
              ></button>
              <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
                <button
                  class="btn btn-outline-dark shadow-none"
                  v-on:click="updateItem(listItem.id)"
                  data-toggle="modal"
                  data-target="#exampleModal"
                  autofocus
                >
                  <i class="material-icons">create</i> Edit
                </button>
                <br />
                <hr />
                <button
                  class="btn btn-outline-danger shadow-none"
                  v-on:click="deleteItem(listItem.id)"
                >
                  <i class="material-icons">delete</i> Delete
                </button>
              </div>
            </div>
          </div>
          <!-- display only on medium(or +) sized devices -->
          <div class="col-3 d-none d-md-block text-right">
            <button
              class="btn btn-outline-dark"
              v-on:click="updateItem(listItem.id)"
              data-toggle="modal"
              data-target="#exampleModal"
              autofocus
            >
              <i class="material-icons">create</i>
            </button>
            <button class="btn btn-outline-danger" v-on:click="deleteItem(listItem.id)">
              <i class="material-icons">delete</i>
            </button>
          </div>
        </div>
      </li>
    </ul>
    <!-- Modal -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">
              <i class="material-icons">create</i> Edit
            </h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <!-- Edit item here -->
          <div class="modal-body">
            <input
              type="text"
              class="form-control form-control-lg pl-2"
              v-model="editItemMsg"
              autofocus
            />
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-primary"
              data-dismiss="modal"
              v-on:click="saveEdit()"
            >Save</button>
            <button type="button" class="btn btn-outline-danger" data-dismiss="modal">Cancel</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    list: {
      type: Array,
      required: true
    }
  },
  methods: {
    deleteItem: function(id) {
      console.log(id);
      this.$emit("deleteItem", id);
    },
    updateItem(id) {
      console.log("Updating item with id: " + id);
      let item = this.list.filter(item => item.id === id)[0];
      console.log(item.message);
      this.editItemMsg = item.message;
      this.editItemId = id;
    },
    saveEdit() {
      console.log(this.editItemId, this.editItemMsg);
      this.$emit("saveEdit", {
        id: this.editItemId,
        updatedMsg: this.editItemMsg
      });
    },
    completedItem: function(item) {
      this.$emit("updateUI", item.id);
    }
  },
  data() {
    return {
      editItemId: null,
      editItemMsg: null
    };
  }
};
</script>

<style scoped>
ul {
  padding: 0px;
}
ul li {
  list-style-type: none;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  padding: 1rem;
}
li:nth-child(odd) {
  background: #f2e7ff;
}
li:nth-child(even) {
  background-color: #ebdbff;
}
label:hover {
  cursor: pointer;
}
label {
  margin: 0 1rem;
  font-size: 20px;
}
input[type="checkbox"] {
  width: 22px;
  height: 22px;
  position: relative;
  top: 6px;
}
.checked {
  opacity: 0.5;
  text-decoration: line-through;
}

@media screen and (max-width: 762px) {
  .fullList {
    padding-top: 15px;
  }
  input[type="checkbox"] {
    width: 20px;
    height: 20px;
    position: relative;
    top: 3px;
  }
  label {
    margin: 0 0.25rem;
    font-size: 18px;
  }
  .dropdown-menu {
    /* left: -83px !important; */
    top: 20px !important;
  }
  .dropdown-menu > button {
    width: 100%;
  }
}
@media screen and (min-width: 763px) {
  .fullList {
    padding-top: 15px;
    width: 75%;
    margin-right: auto;
    margin-left: auto;
  }
}
</style>
