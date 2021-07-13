<template>
  <div class="container">
  
  <header class="row header">
    <div class="logo">Renskiy</div>
    <h1>Канбан доска</h1>
    <div class="mode">
      <img id="mode1" @click="switchMode" src="./assets/moon.svg" alt="">
      <img id="mode2" @click="switchMode" src="./assets/sun.svg" alt="">
    </div>
  </header>
  
    <div class="row  mt-5">
      <div class="col form-inline">
        <b-form-input
          v-bind:class="{ dark: darkMode }"
          id="input-2"
          v-model="newTaskName"
          required
          placeholder="Название"
          @keyup.enter="add"
        ></b-form-input>
        <b-form-input
          v-bind:class="{ dark: darkMode }"
          id="input-3"
          v-model="newTaskText"
          required
          placeholder="Текст"
          @keyup.enter="add"
        ></b-form-input>
        <b-button @click="add" variant="primary" class="ml-3">Добавить</b-button>
      </div>
    </div>
    <div class="row mt-5">
    
      <div class="col-4">
        <div class="p-2 alert alert-secondary">
          <h2>Задачи</h2>
          <draggable
            class="list-group kanban-column"
            :list="arrBackLog"
            group="tasks"
          >
            <div
              v-bind:class="{ dark: darkMode }"
              class="list-group-item"
              v-for="(element, index) in arrBackLog"
              :key="element.name"
            >
              <h3>{{ element.name }}</h3>
              <p>{{ element.text }}</p>
              <div class="edit"><img @click="showPopup(index, 'arrBackLog')" src="./assets/edit.svg" alt=""></div>
            </div>
          </draggable>
        </div>
      </div>

      <div class="col-4">
        <div class="p-2 alert alert-primary">
          <h2>В процессе</h2>
          <draggable
            class="list-group kanban-column"
            :list="arrInProgress"
            group="tasks"
          >
            <div
              v-bind:class="{ dark: darkMode }"
              class="list-group-item"
              v-for="(element, index) in arrInProgress"
              :key="element.name"
            >
              <h3>{{ element.name }}</h3>
              <p>{{ element.text }}</p>
              <div class="edit"><img @click="showPopup(index, 'arrInProgress')" src="./assets/edit.svg" alt=""></div>
            </div>
          </draggable>
        </div>
      </div>

      <div class="col-4">
        <div class="p-2 alert alert-success">
          <h2>Завершено</h2>
          <draggable
            class="list-group kanban-column"
            :list="arrDone"
            group="tasks"
          >
            <div
              v-bind:class="{ dark: darkMode }"
              class="list-group-item"
              v-for="(element, index) in arrDone"
              :key="element.name"
            >
              <h3>{{ element.name }}</h3>
              <p>{{ element.text }}</p>
              <div class="edit">
                <img @click="deleteElement(index)" src="./assets/delete.svg" alt="">
                <img @click="showPopup(index, 'arrDone')" src="./assets/edit.svg" alt="">
              </div>
              
            </div>
          </draggable>
        </div>
      </div>
      
    </div>
    
    <div v-bind:class="{ dark: darkMode }" class="shadow">
      <form v-bind:class="{ dark: darkMode }" class="form">
        <div @click="closePopup" class="cross">+</div>
        <h3>Редактировать запись</h3>
        <label for="name">Название</label>
        <input v-bind:class="{ dark: darkMode }" v-model="editTaskName" required type="text" id="name" placeholder="Название">
        <label for="text">Текст</label>
        <input v-bind:class="{ dark: darkMode }" v-model="editTaskText" required type="text" id="text" placeholder="Текст">
        <button @click="edit">Изменить</button>
      </form>
    </div>
    
    <footer>
      <h2>Хусаинов Ренат Ришатович 201-321</h2>
    </footer>
    
  </div>
</template>

<script>
//import draggable
import draggable from "vuedraggable";

export default {
  name: "kanban-board",
  components: {
    //import draggable as a component
    draggable
  },
  data() {
    return {
      // for new tasks
      newTaskName: "",
      newTaskText: "",
      editTaskName: "",
      editTaskText: "",
      indexOfEdited: 0,
      array: '',
      darkMode: false,
      // 4 arrays to keep track of our 4 statuses
      arrBackLog: [
        { name: "Здрасьте", text: 'Помыть посуду раз два раз два' },
        { name: "Привет", text: 'Помыть посуду раз три сорок шесть' },
      ],
      arrInProgress: [],
      arrDone: []
    };
  },
  methods: {
    //add new tasks method
    add: function() {
      if (this.newTaskName && this.newTaskText) {
        this.arrBackLog.push({ name: this.newTaskName, text: this.newTaskText });
        this.newTaskName = "";
        this.newTaskText = "";
      }
    },
    edit: function() {
      if (this.editTaskName && this.editTaskText) {
        this[this.array][this.indexOfEdited].name = this.editTaskName;
        this[this.array][this.indexOfEdited].text = this.editTaskText;
        this.editTaskName = '';
        this.editTaskText = '';
        document.getElementsByClassName('shadow')[0].classList.remove('active');
      }
    },
    showPopup: function(index, array) {
      document.getElementsByClassName('shadow')[0].classList.add('active');
      this.indexOfEdited = index;
      this.array = array;
    },
    closePopup: function() {
      document.getElementsByClassName('shadow')[0].classList.remove('active');
    },
    deleteElement: function(index) {
      this.arrDone.splice(index, 1);
    },
    switchMode: function() {
      if (!this.darkMode) {
        this.darkMode = true;
        document.getElementById('mode1').style.display = 'none';
        document.getElementById('mode2').style.display = 'inline-block';
      } else {
        this.darkMode = false;
        document.getElementById('mode1').style.display = 'inline-block';
        document.getElementById('mode2').style.display = 'none';
      }
      
      document.getElementsByTagName('body')[0].classList.toggle('dark');
    }
  }
};
</script>

<style>
.kanban-column {
  min-height: 300px;
}
.edit {
    text-align: right;
}
.edit img {
    width: 30px;
    cursor: pointer;
}

.list-group-item {
  cursor: grab;
}
.list-group-item:focus, 
.list-group-item:active {
  cursor: grabbing;
}

.shadow {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  margin: auto;
  background: rgba(0,0,0,0.3);
  opacity: 0;
  z-index: -1;
  transition: .3s;
}

.shadow.dark {
  background: rgba(255,255,255,0.3);
}

.shadow.active {
  z-index: 1;
  opacity: 1;
}

.form {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  margin: auto;
  max-width: 300px;
  height: 400px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background: #fff;
  padding: 25px;
}

.cross {
    position: absolute;
    right: 20px;
    top: 0px;
    font-size: 4rem;
    font-weight: 100;
    transform: rotate(45deg);
    cursor: pointer;
}

.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.header img {
    width: 30px;
    cursor: pointer;
}

#mode2 {
  display: none
}

.dark {
  background-color: #000;
  color: #fff;
}

.list-group-item.dark {
  background-color: #000;
  color: #fff;
}

.form.dark {
  background-color: #000;
}

input.dark {
  background: grey;
  color: #fff;
}

input.dark::placeholder {
  color: #fff;
  opacity: 0.9;
}

footer {
    text-align: center;
}
</style>
