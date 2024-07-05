# Description:
Building a To-Do List application using ES6 JavaScript involves creating a user interface where users can:
Add tasks: Input a task and press a button to add it to the list.
Mark tasks as completed: Toggle the completion status of tasks.
Delete tasks: Remove tasks from the list.
Store tasks: Persist tasks in the browser's local storage to retain them on page reload.

# program:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>To-Do List Application</title>
  <style>
    /* CSS styles */
    body {
      font-family: Arial, sans-serif;
      background-color: #f0f0f0;
      background-image: url('https://www.visiondesign.com/wp-content/uploads/to-do-list-background.jpg');
        background-size: cover; 
        background-repeat: no-repeat; 
        height: 100vh; 
        margin: 0; 
        padding: 0;
      padding: 20px;
    }
    .container {
      max-width: 600px;
      margin: 0 auto;
      background-color: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    .task {
      margin-bottom: 10px;
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 4px;
      background-color: #f9f9f9;
    }
    .task.completed {
      text-decoration: line-through;
      opacity: 0.6;
    }
    form {
      margin-bottom: 20px;
    }
    form input, form textarea, form button {
      margin-bottom: 10px;
      width: 100%;
      padding: 8px;
      font-size: 16px;
      border: 1px solid #ddd;
      border-radius: 4px;
      box-sizing: border-box;
    }
    form button {
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
    form button:hover {
      background-color: #45a049;
    }
  </style>
</head>
<body>

<div class="container">
  <h1>To-Do List</h1>

  <form id="taskForm">
    <input type="text" id="titleInput" placeholder="Enter task title" required>
    <textarea id="descriptionInput" placeholder="Enter task description"></textarea>
    <input type="date" id="dueDateInput">
    <button type="submit">Add Task</button>
  </form>

  <div id="taskList">
    <!-- Tasks will be dynamically added here -->
  </div>

</div>

<script>
  // JavaScript logic
  class Task {
    constructor(id, title, description, dueDate, completed = false) {
      this.id = id;
      this.title = title;
      this.description = description;
      this.dueDate = dueDate;
      this.completed = completed;
    }
  }

  class ToDoList {
    constructor() {
      this.tasks = JSON.parse(localStorage.getItem('tasks')) || [];
      this.taskList = document.getElementById('taskList');
      this.taskForm = document.getElementById('taskForm');

      this.taskForm.addEventListener('submit', this.addTask.bind(this));
      this.renderTasks();
    }

    addTask(event) {
      event.preventDefault();

      const title = document.getElementById('titleInput').value;
      const description = document.getElementById('descriptionInput').value;
      const dueDate = document.getElementById('dueDateInput').value;

      if (!title) {
        alert('Please enter a task title');
        return;
      }

      const id = Date.now().toString();
      const task = new Task(id, title, description, dueDate);

      this.tasks.push(task);
      this.saveTasks();
      this.renderTasks();

      this.taskForm.reset();
    }

    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }

    renderTasks() {
      this.taskList.innerHTML = '';
      this.tasks.forEach(task => {
        const taskElement = document.createElement('div');
        taskElement.className = `task ${task.completed ? 'completed' : ''}`;
        taskElement.innerHTML = `
          <h3>${task.title}</h3>
          <p>${task.description}</p>
          <p>Due Date: ${task.dueDate}</p>
          <button onclick="app.deleteTask('${task.id}')">Delete</button>
          <button onclick="app.toggleCompletion('${task.id}')">${task.completed ? 'Mark Incomplete' : 'Mark Complete'}</button>
        `;
        this.taskList.appendChild(taskElement);
      });
    }

    deleteTask(taskId) {
      this.tasks = this.tasks.filter(task => task.id !== taskId);
      this.saveTasks();
      this.renderTasks();
    }

    toggleCompletion(taskId) {
      const task = this.tasks.find(task => task.id === taskId);
      if (task) {
        task.completed = !task.completed;
        this.saveTasks();
        this.renderTasks();
      }
    }
  }

  const app = new ToDoList();
</script>

</body>
</html>
```
# Output:
![image](https://github.com/bharathipriyan08/to-do-list/assets/113762012/bb2f9e39-f0f9-4a6c-ad00-7be1f4b6974d)
