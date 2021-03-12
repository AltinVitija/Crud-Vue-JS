<template>
  <div id="app">
    <b-container class="bv-example-row">
      <h1 style="margin-bottom: 100px; font-weight: bold">CRUD IN VUE JS</h1>
      <h3 style="margin-right: 930px; font-weight: bold; margin-bottom: 50px">
        Students List
      </h3>
      <b-row>
        <b-col cols="12" md="8" style="padding-right: 55px">
          <table
            v-for="todo in todos"
            :key="todo.id"
            class="table white-sm table-white"
          >
            <thead>
              <tr>
                <th scope="col">Name</th>
                <th scope="col">Surname</th>
                <th scope="col">Birthday</th>
                <th scope="col">Gender</th>
                <th scope="col">Place</th>
                <!-- <b-button @click="deleteTodo(todo.id)" variant="secondary"
                  >Edit</b-button
                > -->
                <b-button
                  @click="deleteTodo(todo.id)"
                  variant="danger"
                  style="margin: 10px"
                  >Delete</b-button
                >
              </tr>
            </thead>
            <tbody>
              <tr>
                <td @dblclick="editTodo(todo)">{{ todo.name }}</td>
                <td>{{ todo.surname }}</td>
                <td>{{ todo.birthday }}</td>
                <td>{{ todo.gender }}</td>
                <td>{{ todo.selected }}</td>
              </tr>
            </tbody>
          </table>
        </b-col>
        <b-col cols="8" md="4" style="margin-top: -80px">
          <h3 style="margin-bottom: 30px; font-weight: bold">
            Create new Student
          </h3>

          <label @dblclick="editTodo(todo)">{{ todo.name }}</label>
          <label style="margin-right: 310px; font-weight: bold">Name</label>

          <b-form-input
            style="margin-bottom: 20px"
            @keyup.enter="addTodo"
            v-model="name"
            placeholder="Enter your name"
          ></b-form-input>
          <label style="margin-right: 290px; font-weight: bold">Surname</label>
          <b-form-input
            style="margin-bottom: 20px"
            v-model="surname"
            placeholder="Enter your Surname"
          ></b-form-input>
          <label
            for="example-datepicker"
            style="margin-bottom: 10px; margin-right: 200px; font-weight: bold"
            >Choose a Birthday</label
          >
          <b-form-datepicker
            style="margin-bottom: 20px"
            id="example-datepicker"
            v-model="birthday"
            class="mb-2"
          ></b-form-datepicker>
          <label
            for="example-datepicker"
            style="margin-bottom: 15px; margin-right: 205px; font-weight: bold"
            >Choose a Gender</label
          >

          <b-form-checkbox-group
            style="
              margin-bottom: 20px;
              margin-right: 100px;
              display: flex;
              flex-direction: row-reverse;
            "
            id="checkbox-group-1"
            v-model="gender"
            :options="options"
            :aria-describedby="ariaDescribedby"
            name="flavour-1"
          >
            <b-form-radio
              style="margin-left: 100px"
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
          <label
            for="example-datepicker"
            style="margin-bottom: 10px; margin-right: 220px; font-weight: bold"
            >Choose a Place</label
          >
          <b-form-select
            style="margin-bottom: 40px"
            v-model="selected"
            :options="place"
          ></b-form-select>

          <button
            style="width: 350px"
            @click="addTodo(todo.id)"
            type="button"
            class="btn btn-primary"
          >
            Save
          </button>
        </b-col>
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
      todos: [],
      place: [
        { value: "Prishtinë", text: "Prishtinë" },
        { value: "Pejë", text: "Pejë" },
        { value: "Mitrovicë", text: "Mitrovicë" },
        { value: "Gjilan  ", text: "Gjilan" },
      ],
    };
  },
  methods: {
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
    editTodo(todo) {
      this.editTodo = todo;
    },
    doneEdit(todo) {
      if (!this.editTodo) {
        return;
      }
      this.editTodo = null;
      todo.name = todo.name.trim();
      if (!todo.name) {
        this.deleteTodo(todo);
      }
    },
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