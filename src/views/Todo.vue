<template>
  <div class="home">
    <v-expand-transition>
      <v-card v-if="hasError" class="mx-auto" color="primary" width="200px">
        <v-card-text>
          <p class="text-h5 mb-6 text-center white--text">{{ error }}</p>
        </v-card-text>
      </v-card>
    </v-expand-transition>
    <v-overlay v-if="hasError" :value="overlay"> </v-overlay>
    <v-text-field
      v-model="newTaskTitle"
      @click:append="addTask"
      @keyup.enter="addTask"
      class="pa-3"
      outlined
      label="Add Task"
      append-icon="mdi-plus"
      hide-details
      clearable
    ></v-text-field>

    <v-card v-if="checkTasks">
      <v-list-item two-line>
        <v-list-item-content>
          <div class="overline mb-4">No Tasks</div>
          <v-list-item-title class="headline mb-1">
            Please add a task.
          </v-list-item-title>
        </v-list-item-content>

        <v-icon color="green lighten-1 text-center" x-large
          >mdi-folder-remove-outline
        </v-icon>
      </v-list-item>
    </v-card>

    <v-list flat pt-0>
      <v-slide-y-transition group>
        <div v-for="task in tasks" :key="task.id">
          <v-list-item
            :key="task.id"
            @click="doneTask(task.id)"
            :class="{ 'blue lighten-5': task.done }"
          >
            <template v-slot:default>
              <v-list-item-action>
                <v-checkbox
                  :input-value="task.done"
                  color="primary"
                ></v-checkbox>
              </v-list-item-action>

              <v-list-item-content>
                <v-list-item-title
                  :class="{ 'text-decoration-line-through': task.done }"
                >
                  {{ task.title }}
                </v-list-item-title>
              </v-list-item-content>
              <v-list-item-action>
                <v-btn icon @click.stop="deleteTask(task.id)">
                  <v-icon color="primary lighten-1">mdi-delete</v-icon>
                </v-btn>
              </v-list-item-action>
            </template>
          </v-list-item>

          <v-divider></v-divider>
        </div>
      </v-slide-y-transition>
    </v-list>
  </div>
</template>

<script>


export default {
  name: 'Todo',
  data () {
    return {
      newTaskTitle: '',
      error: 'Field cannot be empty',
      hasError: false,
      overlay: false,
      noTasks: false,
      tasks: [
        // {
        //   id: 1,
        //   title: 'Feed The Dog',
        //   done: false
        // },
        //  {
        //   id: 2,                          
        //   title: 'Walk The Dog',
        //   done: true 
          
        // },
        //  {
        //   id: 3,
        //   title: 'Train The Dog',
        //   done: false  
        // },
      ]
    }
  },
  methods: {
    addTask(){
      
        let newTask = {
          id: Date.now(),
          title: this.newTaskTitle,
          done: false
        }
        if(!newTask.title) {
          this.hasError = true
          this.overlay = true
          return
        }
        this.tasks.push(newTask);
        this.newTaskTitle = ''
    },
    doneTask(id) {
      let task = this.tasks.filter(task => task.id === id)[0];
      task.done = !task.done
      console.log(task.done)
    },
    deleteTask(id) {
    this.tasks = this.tasks.filter(task => task.id != id)
    }
  },
  computed: {
      checkTasks() {
        if(this.tasks.length === 0) {
          return true
        }
        return false
      }
  },

   watch: {
      overlay (val) {
        val && setTimeout(() => {
          this.overlay = false
          this.hasError = false
        }, 1300)
      },
    
    },
}
</script>
