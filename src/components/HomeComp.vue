<template>
  <main class="container">
    <div class="minimal-width-box">
      <div class=""><h1 class="text-white">TODO <IconMoon v-if="currentTheme === 'index' " class="float-end" @click="changeTheme('dark')" /><IconSun v-if="currentTheme === 'dark' " @click="changeTheme('index')" class="float-end"/></h1></div>

      <div class=" basic-block" @submit="createTodo"><input type="text" v-model="newTask" @keyup.enter="addTask" placeholder="Create a new todo..." class="form-control" id="newTaskInput" /></div>

      <div class="card basic-block shadow-lg">

        <draggable class="list-group dragArea list-group w-full" :list="allTodo">
            <li v-for="item in currentlyLoaded" :key="item.id" class="list-group-item action-item"><span><IconCheck v-if="item.completed" /><input v-else type="radio" :id="[item.id]" class="action-item form-check-input" @click="completeTask(item)" /></span> <label :for="[item.id]" class="action-item"><span :class="{'completed':item.completed}">{{item.item}}</span></label> <IconCross v-if="!item.completed" @click="removeTask(item)" class="float-end" /> </li>
        </draggable>

        <section class="spacing-block">
        <div class="bottom-bar row">
          <ul class="col-md-3 col-sm-5 col-5 col-lg-3 ck-block muted-txt">{{ remainingCount }} items left</ul>
            <ul class="menu text-center d-none d-md-inline-block col-md-5  col-lg-5">
              <li :class="{ 'active': isActive === 1, 'list-group-ietm muted-txt action-item capitalize':listItem }" @click="allTasks">all</li>
              <li :class="{ 'active': isActive === 2, 'list-group-ietm muted-txt action-item capitalize':listItem }" @click="activeTasks">active</li>
              <li :class="{ 'active': isActive === 3 , 'list-group-ietm muted-txt action-item capitalize':listItem}" @click="completedTasks">completed</li>
            </ul>
          <ul class="col-md-4 col-sm-7 col-7 col-lg-4 float-end basc-stack-block muted-txt action-item capitalize" @click="clearCompleted">clear completed</ul>
        </div>
        </section>
      </div>

      <div class="card d-sm-block d-xs-block d-md-none d-lg-none bottom-bar shadow-lg">
        <ul class="menu text-center">
          <li :class="{ 'active': isActive === 1, 'list-group-ietm muted-txt action-item capitalize':listItem }" @click="allTasks">all</li>
          <li :class="{ 'active': isActive === 2, 'list-group-ietm muted-txt action-item capitalize':listItem }" @click="activeTasks">active</li>
          <li :class="{ 'active': isActive === 3, 'list-group-ietm muted-txt action-item capitalize':listItem }" @click="completedTasks">completed</li>
        </ul> 
      </div>
      
      <p class="text-center muted-txt">{{dragDropText}}</p>
    </div>

  </main>

  <bottom>

  </bottom>

</template>

<script>
import { VueDraggableNext } from "vue-draggable-next";
import IconMoon from "@/components/icons/IconMoon.vue";
import IconCheck from "@/components/icons/IconCheck.vue";
import IconCross from "@/components/icons/IconCross.vue";
import IconSun from "@/components/icons/IconSun.vue";
import changeBodyClass from "@/assets/customJs.js";
var id;
id = 1;
export default {
  name: 'HomeComp',
  components: {
    IconMoon,
    IconCheck,
    IconCross,
    IconSun,
    draggable: VueDraggableNext,
  },
  mounted() {
    this.currentlyLoaded = this.allTodo;
  },
  data (){
    return {
      currentTheme: "index",
      dragDropText: "Drag and drop to reorder list",
      currentlyLoaded: [],
      newTask: "",
      isActive: 1,
      listItem: true,
      allTodo: [
        {id: id++, item: "complete online javascript course", completed:true},
        {id: id++, item: "Jog around the park 3x", completed:false},
        {id: id++, item: "10 minutes meditation", completed:false},
        {id: id++, item: "Read for 1 hour", completed:false},
        {id: id++, item: "Pick groceries", completed:false},
        {id: id++, item: "Complete Todo App on Frontend Mentor", completed:false},
      ],
      enabled: true,
      dragging: false,
    }
  },
  

  computed: {

    remainingCount() {
      return this.allTodo.filter((item) => {
            return !item.completed;
          }).length;
      },
   
  },

  methods: {
    addTask: function() {
      if (this.newTask) {
        this.allTodo.push({
          id: id++,
          item: this.newTask,
          completed: false
        });
        this.newTask = "";
      }
  },

  completeTask(task) {
      task.completed = ! task.completed;
    },

  removeTask(task) {
      this.allTodo = this.allTodo.filter((item) => {
        return item.id != task.id;
      });
      this.currentlyLoaded = this.allTodo;
    },

  clearCompleted() {
      this.allTodo = this.allTodo.filter((item) => {
        return !item.completed;
      });
      this.currentlyLoaded = this.allTodo;
  },

  completedTasks() {
    this.currentlyLoaded = this.allTodo.filter((item) => {
        return item.completed;
    });
    this.isActive = 3;
    this.dragDropText = "";
  },
 allTasks() {
    this.currentlyLoaded = this.allTodo;
    this.isActive = 1;
    this.dragDropText = "Drag and drop to reorder list";
  },

  activeTasks() {
    this.currentlyLoaded = this.allTodo.filter((item) => {
        return !item.completed;
    });

    this.isActive = 2;
    this.dragDropText = "";
  },

  changeTheme(theme) {
    this.currentTheme = theme;
    changeBodyClass(theme);
    this.$emit('theme', theme);
  },
}

}


</script>