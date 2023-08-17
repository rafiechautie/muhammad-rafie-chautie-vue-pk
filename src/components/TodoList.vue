<template>
  <div class="todo-list">
    <h1>Todo List</h1>
    <input v-model="newTask" @keyup.enter="addTask" placeholder="Add a new task" />
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        {{ task }}
        <button @click="removeTask(index)">Remove</button>
      </li>
    </ul>
    <div class="quote">
      <p>{{ quote }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [],
      quote: ""
    };
  },
  methods: {
    addTask() {
      if (this.newTask) {
        this.tasks.push(this.newTask);
        this.newTask = '';
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    fetchQuote() {
      fetch("https://type.fit/api/quotes")
        .then(response => response.json())
        .then(data => {
          const randomIndex = Math.floor(Math.random() * data.length);
          this.quote = data[randomIndex].text;
        })
        .catch(error => {
          console.error("Error fetching quote:", error);
        });
    }
  },
  created() {
    this.fetchQuote();
  }
};
</script>

<style scoped>
.todo-list {
  font-family: Arial, sans-serif;
  max-width: 400px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

button {
  background-color: #e74c3c;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 3px;
  cursor: pointer;
}
.quote {
  margin-top: 20px;
  padding: 10px;
  background-color: #f9f9f9;
  border-radius: 5px;
  text-align: center;
  font-style: italic;
}
</style>