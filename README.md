const input = document.getElementById("taskInput");
const button = document.getElementById("addTask");
const list = document.getElementById("taskList");

// جلب المهام من التخزين
let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

// دالة عرض المهام
function renderTasks() {
  list.innerHTML = "";

  tasks.forEach((task, index) => {
    const li = document.createElement("li");
    li.textContent = task;

    // زر حذف
    const deleteBtn = document.createElement("button");
    deleteBtn.textContent = "❌";
    deleteBtn.style.marginLeft = "10px";

    deleteBtn.onclick = () => {
      tasks.splice(index, 1);
      localStorage.setItem("tasks", JSON.stringify(tasks));
      renderTasks();
    };

    li.appendChild(deleteBtn);
    list.appendChild(li);
  });
}

// إضافة مهمة
button.addEventListener("click", () => {
  const taskText = input.value.trim();
  if (taskText === "") return;

  tasks.push(taskText);

  // حفظ في localStorage
  localStorage.setItem("tasks", JSON.stringify(tasks));

  input.value = "";
  renderTasks();
});

// عرض المهام عند فتح الصفحة
renderTasks();
