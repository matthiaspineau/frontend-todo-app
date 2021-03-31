<template>
  <div id="app">
    <div class="wrapper" :class="[ theme == 'light' ? 'theme__light' : 'theme__dark' ]">
      <div class="container wrapper__page"
      :class="[ theme == 'light' ? 'theme__light' : 'theme__dark' ]">

        <div class="container__sm">

          <!-- Sidebar -->
          <header class="header">
            <h1>Todo app</h1>
            <div class="btn__theme"
            @click="changeTheme">
              <img v-if="theme == 'light'" :src="require('@/assets/images/icon-sun.svg')" alt="icon sun">
              <img v-if="theme == 'dark'" :src="require('@/assets/images/icon-moon.svg')" alt="icon moon">
            </div>
          </header>

          <!-- Form user -->
          <div class="wrapper__form">
            <div class="form">
              <input type="checkbox" disabled>
              <input type="text" 
                v-model="newTask"
                placeholder="Create a new todo" 
                @keyup.enter="createNewTask">
            </div>
          </div>

          <!-- Main -->
          <main class="wrapper__main main">
            
            <!-- Top -->
            <div class="main__top">
              <input type="checkbox" checked disabled>
              <label class="label completed">Complete online Javascript course</label>
            </div>

            <!-- Mid -->
            <div class="main__mid">
              <div v-for="(item, index) in filtreItemsComputed" 
                  :key="index" 
                  class="item">
                <input type="checkbox" v-model="item.completed">
                <label class="label" :class="{completed : item.completed}">{{ item.name }}</label>
                <img class="img__cross" 
                @click="removeTask(item)"
                :src="require('@/assets/images/icon-cross.svg')" alt="image cross">
              </div>
            </div>

            <div class="main__bot searchbar searchbar__lg">
              <span class="searchbar__left">{{ filtreItemsComputed.length }} items</span>
              <ul>
                <li @click="filteredItems('all')" :class="{ active : filtered == 'all' }">All</li>
                <li @click="filteredItems('active')" :class="{ active : filtered == 'active' }">Active</li>
                <li @click="filteredItems('completed')" :class="{ active : filtered == 'completed' }">Completed</li>
              </ul>
              <span class="searchbar__right" @click="clearCompleted">Clear Completed</span>
            </div>

          </main>

          <!-- Bot -->
          <div class="searchbar searchbar__sm">
            <ul>
              <li @click="filteredItems('all')" :class="{ active : filtered == 'all' }">All</li>
              <li @click="filteredItems('active')" :class="{ active : filtered == 'active' }">Active</li>
              <li @click="filteredItems('completed')" :class="{ active : filtered == 'completed' }">Completed</li>
            </ul>
          </div>


        </div>

      </div>
    </div>
  </div>
</template>

<script>

const tab = [
  {
    completed: false,
    name: 'tache 1',
  },
   {
    completed: true,
    name: 'tache 2',
  },
   {
    completed: false,
    name: 'tache 3',
  }
]

export default {
  name: 'App',
  data() {
    return {
      theme: 'light',
      newTask: '',
      items: tab,
      itemsFiltered: tab,
      filtered: 'all',
    }
  },
  computed: {
    filtreItemsComputed: {

      get: function () {
        let tab = []
        if (this.filtered == 'all') {
          tab = this.items
        } else if (this.filtered == 'active') {
          tab = this.items.filter( (item) => {
            return item.completed == false
          })
        } else if (this.filtered == 'completed') {
          tab = this.items.filter( (item) => {
            return item.completed == true
          })
        }
        return tab
      },
      set(newTab) {
        this.items = newTab
      }
      
    },
  },
  methods: {
    filteredItems(filtre) {
      this.filtered = filtre
    },
    removeTask(itemRemove) {
      this.filtreItemsComputed = this.items.filter( (item) => {
          return item !== itemRemove
      })
    },
    clearCompleted() {
      console.log('clear completed')
      this.filtreItemsComputed = this.items.filter( (item) => {
          return item.completed !== true
      })
    },
    createNewTask() {
      if (this.newTask !== '') {
        let task = {};
        task.completed = false;
        task.name = this.newTask;
        this.items.push(task);
        console.log(this.items);
        this.newTask = '';
      } 
    },
    changeTheme() {
      this.theme == 'light' ? this.theme = 'dark' : this.theme = 'light';
    },
  },
  components: {
 
  }
}
</script>

<style src="@/assets/scss/app.scss" lang="scss"></style>
