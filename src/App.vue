<script>
import IconBase from "./components/IconBase.vue";
import IconWrite from "./components/icons/IconWrite.vue";
import IconGarbage from "./components/icons/IconGarbage.vue";
import IconPlus from "./components/icons/IconPlus.vue";
import IconOk from "./components/icons/IconOk.vue";

export default {
  el: "#app",
  components: {
    IconBase,
    IconWrite,
    IconGarbage,
    IconPlus,
    IconOk,
  },

  data() {
    return {
      todos: [
        {
          id: 1,
          text: "Встать",
          time: "08:00",
          isEdit: false,
          done: false,
        },
        { id: 2, text: "Умыться", time: "08:30", isEdit: false, done: false },
        { id: 3, text: "Зарядка", time: "09:00", isEdit: false, done: false },
        { id: 4, text: "Завтрак", time: "09:30", isEdit: false, done: false },
      ],
      newTodo: {},
      visibleNewTodo: false,
    };
  },
  //удаление задачи
  methods: {
    removeItem: function (id) {
      this.todos.splice(id, 1);
    },
    //редактирование задачи
    edit(todo) {
      this.todos.forEach((element) => (element.isEdit = false));
      todo.isEdit = true;
    },
    save(todo) {
      todo.isEdit = false;
      this.sortTodos();
    },
    setDone(id) {
      this.todos[id].done = !this.todos[id].done;
    },
    //Добавление задачи
    addNewTodo() {
      this.newTodo.id = this.todos.length + 1;
      this.newTodo.isEdit = false;
      this.todos.push(this.newTodo);
      this.newTodo = {};
      this.sortTodos();
      this.visibleNewTodo = false;
    },
    openNewTodo() {
      this.visibleNewTodo = true;
    },
    byField(field) {
      return (a, b) => (a[field] > b[field] ? 1 : -1);
    },
    sortTodos() {
      return this.todos.sort(this.byField("time"));
    },
  },
};
</script>

<template>
  <div id="app" class="container">
    <h1>Чек лист</h1>
    <ul class="todoList">
      <li v-for="(todo, id) in todos" :key="todo.id" :class="{ done: todo.done }">
        <!-- Режим сохранения -->
        <template v-if="!todo.isEdit">
          {{ todo.time }} {{ todo.text }}
          <!--      Кнопка для редактирования       -->
          <button @click="edit(todo)">
            <icon-base icon-name="write"><icon-write /></icon-base>
          </button>
          <!-- Кнопка для удаления -->

          <button @click="removeItem(id)">
            <icon-base icon-name="garbage" class="delete-btn"
              ><icon-garbage
            /></icon-base>
          </button>
          <button @click="setDone(id)">
            <icon-base icon-name="ok" class="checkDone-btn"
              ><icon-ok
            /></icon-base>
          </button>
        </template>
        <!--      Режим редактирования       -->
        <template v-else>
          <input v-model="todo.time" type="time" /><input v-model="todo.text" />
          <!-- Кнопка для редактирования -->
          <button @click="save(todo)">
            <icon-base icon-name="ok" class="big-icon"><icon-ok /></icon-base>
          </button>
        </template>
      </li>
    </ul>

    <button class="addTask-btn" @click="openNewTodo" v-if="!visibleNewTodo">
      <icon-base icon-name="plus" class="big-icon" style="margin-top: 10px"
        ><icon-plus
      /></icon-base>
      <p>Добавить задачу</p>
    </button>
    <!-- Инпут для новго задания -->
    <div class="newTask" v-if="visibleNewTodo">
      <input type="time" class="newTask_time" v-model="newTodo.time" />
      <input
        type="text"
        class="newTask_text"
        placeholder="введите задачу"
        v-model="newTodo.text"
      />
      <button @click="addNewTodo">
        <icon-base icon-name="ok" class="big-icon"><icon-ok /></icon-base>
      </button>
    </div>
  </div>
</template>

<style>
.container {
  width: 60%;
  margin: 70px auto;
  font-size: 40px;
}
.todoList__item {
  display: flex;
  flex-direction: row;
  height: 50px;
}

.big-icon {
  width: 50px;
  height: 50px;
}

.delete-btn {
  color: red;
}
.addTask-btn {
  margin-top: 10px;
}
.newTask {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-top: 10px;
}

.addTask-btn {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
}

h1 {
  text-align: center;
}
.newTask_time,
.newTask_text {
  outline: none;
}
.newTask_time {
  min-width: 150px;
}
.newTask_text {
  width: 100%;
  margin: 0 5px;
}
button {
  background-color: transparent;
  border: none;
}
.done {
  text-decoration: line-through;
}
@media screen and (max-width: 630px) {
  .container {
    font-size: 25px;
  }
  .newTask {
    flex-direction: column;
    align-items: flex-start;
    margin-left: 40px;
  }
}
</style>
