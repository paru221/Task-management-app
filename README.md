<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Task Management App</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<div class="container">
    <h1>Task Management Application</h1>

    <div class="task-form">
        <input type="text" id="taskInput" placeholder="Enter task">
        <select id="statusInput">
            <option value="Pending">Pending</option>
            <option value="Completed">Completed</option>
        </select>
        <button onclick="addTask()">Add Task</button>
    </div>

    <ul id="taskList"></ul>
</div>

<script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    background: #f4f4f4;
    margin: 0;
    padding: 20px;
}

.container {
    max-width: 700px;
    margin: auto;
    background: white;
    padding: 20px;
    border-radius: 10px;
}

h1 {
    text-align: center;
    color: #333;
}

.task-form {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
}

input, select {
    padding: 10px;
    flex: 1;
}

button {
    padding: 10px 15px;
    border: none;
    background: royalblue;
    color: white;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background: #274ecf;
}

li {
    list-style: none;
    background: #f9f9f9;
    margin-bottom: 10px;
    padding: 10px;
    border-radius: 5px;
}

.task-actions {
    margin-top: 10px;
}

.delete-btn {
    background: red;
}

.edit-btn {
    background: orange;
}
let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

function saveTasks() {
    localStorage.setItem("tasks", JSON.stringify(tasks));
}

function renderTasks() {
    const taskList = document.getElementById("taskList");
    taskList.innerHTML = "";

    tasks.forEach((task, index) => {
        const li = document.createElement("li");

        li.innerHTML = `
            <strong>${task.name}</strong><br>
            Status: ${task.status}
            <div class="task-actions">
                <button class="edit-btn" onclick="editTask(${index})">Edit</button>
                <button class="delete-btn" onclick="deleteTask(${index})">Delete</button>
            </div>
        `;

        taskList.appendChild(li);
    });
}

function addTask() {
    const taskInput = document.getElementById("taskInput");
    const statusInput = document.getElementById("statusInput");

    if (taskInput.value.trim() === "") {
        alert("Please enter a task");
        return;
    }

    tasks.push({
        name: taskInput.value,
        status: statusInput.value
    });

    saveTasks();
    renderTasks();

    taskInput.value = "";
}

function deleteTask(index) {
    tasks.splice(index, 1);
    saveTasks();
    renderTasks();
}

function editTask(index) {
    const newTask = prompt("Edit Task", tasks[index].name);

    if (newTask) {
        tasks[index].name = newTask;
        saveTasks();
        renderTasks();
    }
}

renderTasks();
