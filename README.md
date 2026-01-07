const input = document.getElementById("taskInput");
const button = document.getElementById("addTask");
const list = document.getElementById("taskList");

let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

function renderTasks() {
  list.innerHTML = "";
  tasks.forEach((task, index) => {
    const li = document.createElement("li");
    li.textContent = task;

    const delBtn = document.createElement("button");
    delBtn.textContent = "❌";
    delBtn.style.marginRight = "10px";
    delBtn.onclick = () => {
      tasks.splice(index, 1);
      saveTasks();
      renderTasks();
    };

    li.prepend(delBtn);
    list.appendChild(li);
  });
}

function saveTasks() {
  localStorage.setItem("tasks", JSON.stringify(tasks));
}

button.addEventListener("click", () => {
  if (input.value.trim() !== "") {
    tasks.push(input.value);
    input.value = "";
    saveTasks();
    renderTasks();
  }
});

renderTasks();
