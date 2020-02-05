<template>
  <div id="app">
    <img id="logo" src="./assets/CTSWLogo03.png">
    <h2>Svelte Todo App</h2>

    <div>
      Selected: <strong>{{ completedItems }}</strong><br>
      All Selected: <strong>{{ allSelected }}</strong><br>
      Indeterminate: <strong>{{ indeterminate }}</strong>
    </div>

    <b-form-input v-model="newItem" placeholder="Insert todo item ..."
      @keydown="add"></b-form-input>
      <br/>
    <b-form-group>
      <b-form-checkbox-group
        id="toDos"
        v-model="completedItems"
        :options="shownToDos"
        name="toDos"
        class="ml-4"
        stacked
      ></b-form-checkbox-group>
    </b-form-group>
      <hr>
      <b-form-checkbox
          v-model="allSelected"
          :indeterminate="indeterminate"
          aria-describedby="toDos"
          aria-controls="toDos"
          @change="toggleAll"
        >
          Select All
        </b-form-checkbox>
        <hr>
      <b-form-radio-group
        id="btn-radios-2"
        v-model="selectedFilter"
        :options="optionsFilter"
        buttons
        button-variant="outline-primary"
        name="radio-btn-outline"
      ></b-form-radio-group>
    <b-button @click="clearCompleted">Clear completed</b-button>
  </div>
</template>

<script>
// import tdItem from './components/todoItem'
import BootstrapVue from 'bootstrap-vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    BootstrapVue, axios
  },
  mounted () {
    // this.toDosArray = getArregloFromAPI();
  },
  data () {
    return {
      apiRoute: '', // the route for an API
      toDosArray: [], // the toDo list
      shownToDos: [], // of the list, the ones that are being shown, according to selected filter
      completedItems: [], // the toDos selected
      newItem: '', // the new toDo
      selectedFilter: 'all', // filter selected by the user
      optionsFilter: [ // the options for the filter
        { text: 'All', value: 'all' },
        { text: 'Active', value: 'active' },
        { text: 'Completed', value: 'comp' }
      ],
      allSelected: false, // whether all toDos are selected
      indeterminate: false // whether NOT all toDos are selected or unselected
    }
  },
  methods: {
    getToDosFromAPI: function () { // this method is to get the list from an API

    },
    toggleAll (checked) { // check or unchecks all items
      var allWereSelected = []
      this.toDosArray.forEach(function (elemento, indice, array) {
        allWereSelected.push(elemento.text)
      })
      this.completedItems = checked ? allWereSelected : []
    },
    add: function (event) { // adds a new item
      if (event.key === 'Enter' && event.target.value.length > 0) {
        this.toDosArray.push(
          { text: this.newItem, value: this.newItem, fecha: new Date() }
        )
        this.newItem = ''
      }
    },
    clearCompleted: function () {
      var completed = this.completedItems
      this.toDosArray = this.toDosArray.filter(function (obj) {
        return completed.indexOf(obj.text) === -1
      })
      this.completedItems = []
    },
    remove: function (event) { // removes a given item  @TODO
      this.toDosArray.forEach(function (elemento, indice, array) {
        if (elemento[name] === event.target.value) {
          alert(indice)
        }
      })
      // this.toDosArray.remove(event.target.value)
    },
    ajax: function () { // API call using axios, NOT USED
      axios({
        method: 'post',
        url: this.apiRoute
      })
        .then(function (response) {
        })
    }
  },
  watch: {
    completedItems (newVal, oldVal) { // sets the selected status of Check All
      if (newVal.length === 0) {
        this.indeterminate = false
        this.allSelected = false
      } else if (newVal.length === this.toDosArray.length) {
        this.indeterminate = false
        this.allSelected = true
      } else {
        this.indeterminate = true
        this.allSelected = false
      }
    },
    selectedFilter (newVal, oldVal) { // filters the shown toDos
      this.shownToDos = []
      var todos = this.toDosArray
      var completed = this.completedItems
      var res = []
      switch (newVal) {
        case 'all':
          res = todos
          break
        case 'active':
          todos.forEach(function (elemento, indice, array) {
            if (completed.indexOf(elemento.text) === -1) {
              res.push(elemento)
            }
          })
          break
        case 'comp':
          todos.forEach(function (elemento, indice, array) {
            if (completed.indexOf(elemento.text) !== -1) {
              res.push(elemento)
            }
          })
          break
      }
      this.shownToDos = res
    },
    toDosArray () {
      this.shownToDos = this.toDosArray // removes filters if new toDo is added
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#logo{
  width: 60%;
}

#toDos {
  margin-left: 0px;
}
</style>
