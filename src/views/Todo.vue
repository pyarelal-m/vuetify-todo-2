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
    <v-virtual-scroll
      :items="tasks"
      :item-height="20"
      height="100"
    >
    <template v-slot:default="{ task }">
      <v-list-item
          @click="doneTask(task.id)"
          :class="{'red': task.done}"
        >
          <!-- <template v-slot:default> -->
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
          <!-- </template> -->
        </v-list-item>
    </template>
    </v-virtual-scroll>
    <v-virtual-scroll
      :items="items"
      :item-height="50"
      height="300"
    >
      <template v-slot:default="{ item }">
        <v-list-item>
          <v-list-item-avatar>
            <v-avatar
              :color="item.color"
              size="56"
              class="white--text"
            >
              {{ item.initials }}
            </v-avatar>
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title>{{ item.fullName }}</v-list-item-title>
          </v-list-item-content>

          <v-list-item-action>
            <v-btn
              depressed
              small
            >
              View User

              <v-icon
                color="orange darken-4"
                right
              >
                mdi-open-in-new
              </v-icon>
            </v-btn>
          </v-list-item-action>
        </v-list-item>
      </template>
    </v-virtual-scroll>
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
      benched: 0,
      newTaskTitle: '',
      // tasks: [],
      colors: ['#2196F3', '#90CAF9', '#64B5F6', '#42A5F5', '#1E88E5', '#1976D2', '#1565C0', '#0D47A1', '#82B1FF', '#448AFF', '#2979FF', '#2962FF'],
      names: ['Oliver', 'Jake', 'Noah', 'James', 'Jack', 'Connor', 'Liam', 'John', 'Harry', 'Callum', 'Mason', 'Robert', 'Jacob', 'Jacob', 'Jacob', 'Michael', 'Charlie', 'Kyle', 'William', 'William', 'Thomas', 'Joe', 'Ethan', 'David', 'George', 'Reece', 'Michael', 'Richard', 'Oscar', 'Rhys', 'Alexander', 'Joseph', 'James', 'Charlie', 'James', 'Charles', 'William', 'Damian', 'Daniel', 'Thomas', 'Amelia', 'Margaret', 'Emma', 'Mary', 'Olivia', 'Samantha', 'Olivia', 'Patricia', 'Isla', 'Bethany'],
      surnames: ['Smith', 'Anderson', 'Clark', 'Wright', 'Mitchell', 'Johnson', 'Thomas', 'Rodriguez', 'Lopez', 'Perez', 'Williams', 'Jackson', 'Lewis', 'Hill', 'Roberts', 'Jones', 'White', 'Lee', 'Scott', 'Turner', 'Brown', 'Harris', 'Walker', 'Green', 'Phillips', 'Davis', 'Martin', 'Hall', 'Adams', 'Campbell', 'Miller', 'Thompson', 'Allen', 'Baker', 'Parker', 'Wilson', 'Garcia', 'Young', 'Gonzalez', 'Evans', 'Moore', 'Martinez', 'Hernandez', 'Nelson', 'Edwards', 'Taylor', 'Robinson', 'King', 'Carter', 'Collins'],
    }
  },
  computed: {
      items () {
        const namesLength = this.names.length
        const surnamesLength = this.surnames.length
        const colorsLength = this.colors.length

        return Array.from({ length: 100 }, (k, v) => {
          const name = this.names[this.genRandomIndex(namesLength)]
          const surname = this.surnames[this.genRandomIndex(surnamesLength)]

          return {
            color: this.colors[this.genRandomIndex(colorsLength)],
            fullName: `${name} ${surname}`,
            initials: `${name[0]} ${surname[0]}`,
          }
        })
      },
      tasks () {
        return []
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
    },
    genRandomIndex (length) {
        return Math.ceil(Math.random() * (length - 1))
      },
  }
};
</script>
