<template>
  <div class="to-do-container">
    <div class="welcome">{{ welcomeMessage }}</div>

    <div class="alert alert-primary" v-show="message">{{ message }}</div>

    <div>
      <div class="form-group row">
        <label for="Name" class="col-sm-1">Title</label>
        <div class="col-sm-4">
          <input
            type="text"
            id="Name"
            v-model="model.Name"
            class="form-control"
          />
        </div>
        <div class="form-check">
          <input
            type="checkbox"
            id="isCompleted"
            v-model="model.IsCompleted"
            class="form-check-input"
          />
          <label for="isCompleted" class="form-check-label">
            Completed
          </label>
        </div>
      </div>
      <div>
        <button type="submit" @click="AddToDo()" class="btn btn-primary">
          Add ToDo
        </button>
      </div>
    </div>

    <section>
      <ol class="list-group">
        <li class="list-group-item" v-for="todo of ToDos" :key="todo.Name">
          {{ todo.Name }}, {{ todo.IsCompleted ? "Completed" : "Not Done" }}
        </li>
      </ol>
    </section>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import ToDoModel from "@/models/ToDoModel";
import store from "@/store";

@Component({
  components: {}
})
export default class ToDoWithoutStore extends Vue {
  private welcomeMessage: string;
  private message: string;
  private model: ToDoModel;
  constructor() {
    super();
    this.welcomeMessage = "Welcome to ToDo App!";
    this.model = new ToDoModel();
    this.message = "";
  }
  get ToDos(): ToDoModel[] {
    console.log(this.$store);
    let todos = this.$store.state.ToDo.todos;
    return todos;
  }
  AddToDo() {
    this.message = `Adding ${this.model.Name} to ToDo List ...`;
    if (this.ToDos.some(x => x.Name == this.model.Name)) {
      this.message = `ToDo item ${this.model.Name} already exists in your list`;
      return;
    }
    this.$store
      .dispatch("ToDo/addToDo", this.model)
      .then(() => {
        this.message = "ToDo added successfully to your list";
        setTimeout(() => {
          this.message = "";
        }, 1500);
      })
      .catch(error => {
        console.error(error);
      });
    // this.todos.push(this.model);
    this.model = new ToDoModel();
  }
}
</script>

<style lang="scss" scoped>
.to-do-container {
  border: 1px solid #cccccc;
  width: 600px;
  margin: 48px auto;
  background: whitesmoke;
  border-radius: 8px;

  .welcome {
    color: #090629e5;
    margin: 20px;
    font-size: 20px;
  }
  .message {
    margin: 20px;
    font-size: 14px;
  }
  section {
    margin: 20px;
  }
  .list-group-item {
    border-radius: 8px;
    background: #fff;
    border: 1px solid #ababab;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 12px 40px;
    height: 48px;
    font-size: 16px;
    color: #5f5f5f;
  }
}
</style>
