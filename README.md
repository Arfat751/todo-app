<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>Todo App</title>

<style>
body {
  margin: 0;
  min-height: 100vh;
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(270deg, #764ba2, #667eea);
  background-size: 800% 800%;
  animation: bgMove 15s ease infinite;
}

@keyframes bgMove {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.app {
  background: rgba(255,255,255,0.9);
  width: 95%;
  max-width: 420px;
  padding: 20px;
  border-radius: 18px;
  box-shadow: 0 20px 40px rgba(0,0,0,0.3);
}

h1 {
  text-align: center;
  margin-bottom: 10px;
  color: #000;
}

.counter {
  text-align: center;
  color: #000;
  margin-bottom: 15px;
  font-size: 14px;
}

/* input + زر الإضافة */
.input-box {
  display: flex;
  flex-direction: row-reverse; /* زر إضافة في اليسار */
  gap: 10px;
}

input {
  flex: 1;
  padding: 12px;
  font-size: 16px;
  border-radius: 10px;
  border: 1px solid #ccc;
  color: #000;
}

input::placeholder {
  color: #555;
}

.add-btn {
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: #fff;
  border: none;
  padding: 12px 16px;
  border-radius: 10px;
  font-size: 16px;
  cursor: pointer;
}

/* قائمة المهام */
ul {
  list-style: none;
  padding: 0;
  margin-top: 20px;
}

li {
  background: #f2f2f2;
  padding: 10px;
  border-radius: 12px;
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  gap: 10px;
}

/* أزرار ✔ ✖ في اليسار */
.actions {
  display: flex;
  gap: 6px;
}

.done-btn,
.delete-btn {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  border: none;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
}

.done-btn { background: #4caf50; }
.delete-btn { background: #ff5252; }

/* نص المهمة */
.task-text {
  flex: 1;
  color: #000;
  text-align: right;
}

li.done .task-text {
  text-decoration: line-through;
  color: #777;
}
</style>
</head>

<body>

<div class="app">
  <h1>🔥 مهامي</h1>
  <div class="counter" id="counter"></div>

  <div class="input-box">
    <input type="text" id="taskInput" placeholder="اكتب مهمة">
    <button class="add-btn" id="addTask">إضافة</button>
  </div>

  <ul id="taskList"></ul>
</div>

<script>
const input = document.getElementById("taskInput");
const button = document.getElementById("addTask");
const list = document.getElementById("taskList");
const counter = document.getElementById("counter");

let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

function saveTasks() {
  localStorage.setItem("tasks", JSON.stringify(tasks));
}

function updateCounter() {
  counter.textContent = "المهام المتبقية: " + tasks.filter(t => !t.done).length;
}

function renderTasks() {
  list.innerHTML = "";
  tasks.forEach((task, index) => {
    const li = document.createElement("li");
    if (task.done) li.classList.add("done");

    const actions = document.createElement("div");
    actions.className = "actions";

    const doneBtn = document.createElement("button");
    doneBtn.className = "done-btn";
    doneBtn.textContent = "✔";
    doneBtn.onclick = () => {
      task.done = !task.done;
      saveTasks();
      renderTasks();
    };

    const deleteBtn = document.createElement("button");
    deleteBtn.className = "delete-btn";
    deleteBtn.textContent = "✖";
    deleteBtn.onclick = () => {
      tasks.splice(index, 1);
      saveTasks();
      renderTasks();
    };

    const span = document.createElement("span");
    span.className = "task-text";
    span.textContent = task.text;

    /* الأزرار أولًا = يسار */
    actions.appendChild(doneBtn);
    actions.appendChild(deleteBtn);
    li.appendChild(actions);
    li.appendChild(span);

    list.appendChild(li);
  });

  updateCounter();
}

button.onclick = addTask;
input.addEventListener("keydown", e => {
  if (e.key === "Enter") addTask();
});

function addTask() {
  const text = input.value.trim();
  if (!text) return;
  tasks.push({ text, done: false });
  saveTasks();
  input.value = "";
  renderTasks();
}

renderTasks();
</script>

</body>
</html>
