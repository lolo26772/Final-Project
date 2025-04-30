# Final-Project

https://wcet.waketech.edu/ypchingombe/WEB115/M15%20Final%20Project.html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Task Manager</title> 
  <link rel="stylesheet" href="styles.css">
<style>
body {
  font-family: Futura, sans-serif;
  margin: 15px;
  background-color: #BE89C9;
}

form {
  margin-bottom: 20px;
}

.task {
  padding: 10px;
  border: 1px solid #ccc;
  margin-bottom: 10px;
  border-radius: 5px;
}
/* style for the checkboxes*/
input[type="checkbox"] {
  width: 15px;
  height: 20px;
  border-radius: 60%; 
  accent-color: #EA8836;
  margin-left: 8px;
}
/* animation for the checkmark*/
.task .checkmark {
  display: inline-block;
  margin-right: 10px;
  color: red;
  font-weight: bold;
  opacity: 0;
  transform: scale(0.5);
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.task.completed .checkmark {
  opacity: 1;
  transform: scale(1.2);
}

/* Red text for important tasks */
.important {
  color: red;
  font-weight: bold;
}

/*style for the completed tasks*/
.task.completed {
  text-decoration: line-through;
  opacity: 0.6;
  transition: all 0.3s ease;
}
</style>
</head>
<body>
  <h1>Task Manager</h1>
  <form id="taskForm">
    <input type="text" id="taskName" placeholder="Enter task name" required>
    <select id="taskPriority">
      <option value="High">High</option>
      <option value="Medium">Medium</option>
      <option value="Low">Low</option>
    </select>
    <label>
      <input type="checkbox" id="taskImportant"> Important
    </label>
    <label>
      <input type="checkbox" id="taskCompleted"> Completed
    </label>
    <button type="submit">Add Task</button>
  </form>

  <div id="taskmanager"></div>

  <script>
const taskForm = document.getElementById('taskForm');
const taskManager = document.getElementById('taskmanager');

/* storage for the tasks*/
let tasks = [];
let taskId = 1;

taskForm.addEventListener('submit', function (e) {
  e.preventDefault();

  const name = document.getElementById('taskName').value.trim();
  const priority = document.getElementById('taskPriority').value;
  const isImportant = document.getElementById('taskImportant').checked;
  const isCompleted = document.getElementById('taskCompleted').checked;

  if (!name) {
    alert("Task name cannot be empty.");
    return;
  }

  const task = {
    id: taskId++,
    name,
    priority,
    isImportant,
    isCompleted,
    date: new Date().toLocaleString()
  };

  tasks.push(task);
  renderTasks();
  console.log(JSON.stringify(tasks, null, 2));
  taskForm.reset();
});

function renderTasks() {
  taskManager.innerHTML = '';

  tasks.forEach(task => {
    const taskDiv = document.createElement('div');
    taskDiv.className = 'task';

    if (task.isImportant) {
      taskDiv.classList.add('important');
    }
    if (task.isCompleted) {
      taskDiv.classList.add('completed');
    }

    taskDiv.innerHTML = `
  <span class="checkmark">${task.isCompleted ? "✔️" : ""}</span>
  <strong>${task.name}</strong> [${task.priority}] - <em>${task.date}</em><br>
  <button onclick="deleteTask(${task.id})">Delete</button>
  <label>
    <input type="checkbox" onchange="toggleComplete(${task.id})" ${task.isCompleted ? 'checked' : ''}>
    Completed
  </label>
  <label>
    <input type="checkbox" onchange="toggleImportant(${task.id})" ${task.isImportant ? 'checked' : ''}>
    Important
  </label>
`;
    taskManager.appendChild(taskDiv);
  });
}

function deleteTask(id) {
  tasks = tasks.filter(task => task.id !== id);
  renderTasks();
  console.log(JSON.stringify(tasks, null, 2));
}

function toggleComplete(id) {
  const task = tasks.find(t => t.id === id);
  if (task) {
    task.isCompleted = !task.isCompleted;
    renderTasks();
    console.log(JSON.stringify(tasks, null, 2));
  }
}

function toggleImportant(id) {
  const task = tasks.find(t => t.id === id);
  if (task) {
    task.isImportant = !task.isImportant;
    renderTasks();
    console.log(JSON.stringify(tasks, null, 2));
  }
}
</script>
</body>
</html>
