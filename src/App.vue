<template>
  <div id="app">
    <b-container class="bv-example-row">
      <h1 style="margin-bottom: 100px; font-weight: bold">CRUD IN VUE JS</h1>
      <h3 style="margin-right: 930px; font-weight: bold; margin-bottom: 50px">
        Students List
      </h3>
      <b-row>
        <b-col cols="12" md="8" style="padding-right: 55px">
          <table class="table white-sm table-white">
            <thead>
              <tr>
                <th scope="col">Name</th>
                <th scope="col">Surname</th>
                <th scope="col">Birthday</th>
                <th scope="col">Gender</th>
                <th scope="col">Place</th>
              </tr>
            </thead>
            <tbody v-for="todo in todos" :key="todo.id">
              <tr>
                <td>{{ todo.name }}</td>
                <td>{{ todo.surname }}</td>
                <td>{{ todo.birthday }}</td>
                <td>{{ todo.gender }}</td>
                <td>{{ todo.selected }}</td>
                <td>
                  <b-button
                    style="margin-right: 5px"
                    @click="showModal"
                    variant="secondary"
                    >Edit</b-button
                  >
                  <b-button @click="deleteTodo(todo.id)" variant="danger"
                    >Delete</b-button
                  >
                </td>
              </tr>
            </tbody>
          </table>
        </b-col>
        <div>
          <b-button id="show-btn" @click="showModal"
            >Create new Student
          </b-button>
        </div>
        <b-modal ref="my-modal" hide-footer title="Create new Student">
          <label style="font-weight: bold">Name</label>
          <b-form-input
            @keyup.enter="addTodo"
            v-model="name"
            placeholder="Enter your name"
          ></b-form-input>
          <label style="margin-right: 290px; font-weight: bold">Surname</label>
          <b-form-input
            v-model="surname"
            placeholder="Enter your Surname"
          ></b-form-input>
          <label for="example-datepicker" style="font-weight: bold"
            >Choose a Birthday</label
          >
          <b-form-datepicker
            id="example-datepicker"
            v-model="birthday"
            class="mb-2"
          ></b-form-datepicker>
          <label for="example-datepicker" style="font-weight: bold"
            >Choose a Gender</label
          >
          <b-form-checkbox-group
            style="
              display: flex;
              flex-direction: row-reverse;
              margin-right: 280px;
            "
            id="checkbox-group-1"
            v-model="gender"
            :options="options"
            :aria-describedby="ariaDescribedby"
            name="flavour-1"
          >
            <b-form-radio
              style="margin-left: 50px"
              v-model="gender"
              :aria-describedby="ariaDescribedby"
              name="some-radios"
              value="Women"
              >Women</b-form-radio
            >
            <b-form-radio
              v-model="gender"
              :aria-describedby="ariaDescribedby"
              name="some-radios"
              value="Men"
              >Men</b-form-radio
            >
          </b-form-checkbox-group>
          <label for="example-datepicker" style="font-weight: bold"
            >Choose a Place</label
          >
          <b-form-select v-model="selected" :options="place"></b-form-select>
          <b-button
            style="width: 470px"
            class="mt-3"
            variant="outline-primary"
            @click="addTodo(todo.id)"
            type="button"
          >
            Save
          </b-button>
        </b-modal>
      </b-row>
    </b-container>
  </div>
</template>


<script>
import { v4 } from "uuid";
export default {
  name: "Home",
  data() {
    return {
      todo: "",
      todoItem: {},
      todos: [],
      place: [
        { value: "Prishtine", text: "Prishtine" },
        { value: "Peje", text: "Peje" },
        { value: "Mitrovice", text: "Mitrovice" },
        { value: "Gjilan  ", text: "Gjilan", disabled: true },
      ],
    };
  },
  methods: {
    showModal() {
      this.$refs["my-modal"].show();
    },
    hideModal() {
      this.$refs["my-modal"].hide();
    },
    addTodo() {
      this.todos.push({
        selected: this.selected,
        id: v4(),
        name: this.name,
        surname: this.surname,
        birthday: this.birthday,
        gender: this.gender,
      });
      this.updateLocalStorage(this.todos);
      this.todo = "";
    },
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
      this.updateLocalStorage(this.todos);
    },
    updateLocalStorage(todos) {
      localStorage.setItem("todos", JSON.stringify(todos));
    },
  },
  edit: function (todos) {
    this.todo.name = todos.name;
    this.todo.surname = todos.surname;
  },
  created() {
    const ls_todos = localStorage.getItem("todos");
    if (ls_todos !== null) this.todos = JSON.parse(ls_todos);
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
</style>