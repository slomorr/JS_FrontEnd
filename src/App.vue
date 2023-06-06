<template>
  <v-app>
    <v-main>
      <v-row>
        <v-col>
            <v-btn
              v-on:click="deleteFields(), create = !create"
              block
              depressed
              color = "#5feb34"
              elevation="19"
              large
            >
              СОЗДАТЬ ToDO
            </v-btn>
            <v-bottom-sheet hide-overlay v-model="create">
              <v-sheet class="text-center" height="200px">
                <v-row>
                  <v-btn
                    v-on:click="createToDo(), create = !create"
                    block
                    depressed
                    color = "#5feb34"
                    elevation="19"
                    large
                  >
                    Создать
                  </v-btn>
                </v-row>
                <v-container>
                  <v-row>
                    <v-col>
                      <v-text-field label="title" v-model="title">
                      </v-text-field>
                    </v-col>
                    <v-col>
                      <v-text-field label="description" v-model="description">
                      </v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
                <v-btn text color="error" @click="create = !create">
                  Закрыть
                </v-btn>
              </v-sheet>
            </v-bottom-sheet>
          </v-col>
        <v-col>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
        <v-btn
          block
          depressed
          elevation="19"
          color = "#eb5234"
          large
          v-on:click="deleteToDoFromList()">
            Удалить ToDoList
          </v-btn>
          </v-col>
          <v-col>
          </v-col>
      </v-row>
      <v-row>
        <v-col>
        <v-btn
          block
          depressed
          color = "#8634eb"
          elevation="19"
          large
          
          v-on:click="refreshData()">
            Показать ToDoList
          </v-btn>
          </v-col>
          <v-col>
          </v-col>
      </v-row>
    </v-main>
  </v-app>
</template>

<script>

import axios from "axios";

export default {

  name: "App",

  created() {
    this.fetchData();
  },
  data: () => {
    return {
      create: false,
      title: "",
      description: "",
      todoList: [],
    };
  },
  methods: {

    setFields(title, description) {
      this.title = title;
      this.description = description;
    },

    setID(id0){
      this.id0 = id0
    },

    deleteFields() {
      this.title = "";
      this.description = "";
    },

    async deleteToDoFromList() {
      await axios
        .delete("http://localhost:3100/api/todo/" + this.id0)
        .catch(function (error) {
          console.log(error);
        });
      this.refreshData();
    },

    async refreshData() {
      const response = await axios.get("http://localhost:3100/api/todo");
      console.log("data todolist", response.data.todoList);
      this.todoList = response.data.todoList;
    },

    async updateToDo() {
      await axios
        .patch("http://localhost:3100/api/todo/" + this.id0, {
          title: this.title,
          description: this.description,
        })
        .catch(function (error) {
          console.log(error);
        });
      this.refreshData();
    },
    async createToDo() {
      await axios
        .post("http://localhost:3100/api/todo", {
          title: this.title,
          description: this.description,
        })
        .catch(function (error) {
          console.log(error);
        });
      this.refreshData();
    },

    async deleteToDoList() {
      await axios
        .delete("http://localhost:3100/api/todo")
        .catch(function (error) {
          console.log(error);
        });
      this.refreshData();
    },

  },
};
</script>