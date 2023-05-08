<template>
  <div class="home">
    <v-text-field
      class="pa-3"
      outlined
      label="Add Task"
      append-icon="mdi-plus"
      hide-details
      clearable
      v-model="newTaskTitle"
      @click:append="addNewTask"
      @keyup.enter="addNewTask"
    ></v-text-field>
    <v-list
    class="pt-0"
    flat
    >
    <!-- https://youtu.be/CjXgoYo86yY?t=1854 -->
      <div v-for="task in tasks" :key="task.id">
        <v-list-item
          @click="doneTask(task.id)"
          :class="{'red': task.done}"
        >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox :input-value="task.done" color="primary"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{'text-decoration-line-through': task.done}"
              >{{task.title}}</v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
              <v-btn
                icon
                @click.stop="deleteTask(task.id)"
              >
                <v-icon color="grey lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>
    </v-list>
  </div>
</template>

<script>
export default {
  
  name: "Home",

  components: {
    // HelloWorld,
  },

  data() {
    return {
      newTaskTitle: '',
      tasks: []
    }
  },
  methods: {
    addNewTask() {
      this.tasks.push({
        id: Date.now(),
        title: this.newTaskTitle,
        done: false
      });
      this.newTaskTitle = ''
      console.log( Object.entries(this.tasks))
    },
    doneTask(id) {
      let task = this.tasks.filter(task => task.id === id)[0]
      task.done = !task.done;
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id)
    }
  }
};
</script>
