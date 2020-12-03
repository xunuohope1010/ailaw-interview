<template>

  <v-container>
    <v-row>
      <v-col cols="11">
        <v-text-field
            filled
            v-model="input"
            label="Add tasks"
            placeholder="Type to add new a task"
        ></v-text-field>

      </v-col>
      <v-col>
        <v-btn
            class="mx-2"
            fab
            dark
            color="indigo"
            @click="addTask"
            :disabled="(this.input==='')"
        >
          <v-icon dark>
            mdi-plus
          </v-icon>
        </v-btn>
      </v-col>

    </v-row>

    <v-row>
      <v-col cols="12">
        <v-simple-table
            fixed-header
            height="500px"
            :key="componentKey"
        >
          <template v-slot:default>
            <thead>
            <tr>
              <th class="text-left">
                Mark
              </th>
              <th class="text-left">
                Edit
              </th>
              <th class="text-left">
                Delete
              </th>
            </tr>
            </thead>
            <tbody>
            <tr
                v-for="item in tasks"
                :key="item"
            >
              <td>
                <v-checkbox
                    v-model="marked"
                    :label="item"
                    :value="item"
                    @change="markTask"
                ></v-checkbox>
              </td>
              <td>
                <v-btn
                    class="mx-2"
                    fab
                    dark
                    small
                    color="cyan"
                    @click="editTask(item)"
                >
                  <v-icon dark>
                    mdi-pencil
                  </v-icon>
                </v-btn>
              </td>
              <td>
                <v-btn
                    class="mx-2 btn"
                    fab
                    dark
                    small
                    color="primary"
                    @click="deleteTask(item)"
                >
                  <v-icon dark>
                    mdi-minus
                  </v-icon>
                </v-btn>
              </td>
            </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-col>
    </v-row>

  </v-container>
</template>

<script>
export default {
  name: "Interview",
  mounted() {
    this.tasks = JSON.parse(localStorage.getItem("tasks") || "[]")
    this.marked = JSON.parse(localStorage.getItem("marked") || "[]")
  },

  data() {
    return {
      input: "",
      marked: [],
      tasks: [],
      componentKey: 0,
    }
  },
  methods: {
    markTask() {
      localStorage.setItem("marked", JSON.stringify(this.marked))
    },
    deleteTask(item) {
      const indexMarked = this.marked.indexOf(item)
      const indexTasks = this.tasks.indexOf(item)
      if (indexMarked >= 0) this.marked.splice(indexMarked, 1)
      if (indexTasks >= 0) this.tasks.splice(indexTasks, 1)
      localStorage.setItem("tasks", JSON.stringify(this.tasks))
      localStorage.setItem("marked", JSON.stringify(this.marked))
    },
    addTask() {
      if (this.input !== "") {
        const indexTasks = this.tasks.indexOf(this.input)
        if (indexTasks !== -1) {
          alert("Task already exist, please choose another name!")
        } else {
          this.tasks.push(this.input)
          localStorage.setItem("tasks", JSON.stringify(this.tasks))
        }
      }
    },
    editTask(item) {
      const indexTasks = this.tasks.indexOf(item)
      const indexMarked = this.marked.indexOf(item)
      const myTask = prompt("Please enter the new name:", "")
      if (myTask === ""||myTask===null) {
        alert("Name cannot be null!")
      } else {
        if(this.tasks.indexOf(myTask)!==-1){
          alert("Name already exist!")
          return
        }
        this.tasks[indexTasks] = myTask
        if (indexMarked !== -1) {
          this.marked[indexMarked] = myTask;
        }
        localStorage.setItem("tasks", JSON.stringify(this.tasks))
        localStorage.setItem("marked", JSON.stringify(this.marked))
        this.componentKey++;
      }
    }
  },


}
</script>

<style scoped>

</style>
