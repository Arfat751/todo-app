<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>تطبيق المهام الجذاب</title>
  <style>
    /* ===== CSS ===== */
    body {
      font-family: 'Arial', sans-serif;
      background: linear-gradient(to bottom, #6a11cb, #2575fc);
      color: #fff;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 0;
    }

    .container {
      background-color: rgba(255, 255, 255, 0.1);
      padding: 30px 40px;
      border-radius: 20px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.3);
      width: 90%;
      max-width: 400px;
      text-align: center;
    }

    h1 {
      margin-bottom: 20px;
    }

    .input-section {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }

    input {
      flex: 1;
      padding: 10px;
      border-radius: 10px;
      border: none;
      outline: none;
      font-size: 16px;
    }

    button {
      padding: 10px 20px;
      background-color: #ff6b6b;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      font-weight: bold;
      transition: 0.3s;
    }

    button:hover {
      background-color: #ff4757;
    }

    ul {
      list-style-type: none;
      padding: 0;
    }

    li {
      background-color: rgba(255, 255, 255, 0.2);
      padding: 10px;
      margin-bottom: 10px;
      border-radius: 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      cursor: pointer;
      transition: 0.3s;
    }

    li.completed {
      text-decoration: line-through;
      background-color: rgba(0, 0, 0, 0.3);
      color: #ccc;
    }

    li button {
      background-color: #ff4757;
      padding: 5px 10px;
      border-radius: 5px;
      font-size: 12px;
    }

    li button:hover {
      background-color: #ff6b6b;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>📝 تطبيق المهام</h1>
    <div class="input-section">
      <input id="taskInput" placeholder="اكتب مهمة جديدة">
      <button id="addTask">إضافة</button>
    </div>
    <ul id="taskList"></ul>
  </div>

  <script>
    // ===== JavaScript =====
    const input = document.getElementById("taskInput");
    const button = document.getElementById("addTask");
    const list = document.getElementById("taskList");

    // جلب المهام من localStorage أو إنشاء مصفوفة جديدة
    let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

    // دالة لعرض المهام
    function renderTasks() {
      list.innerHTML = "";
      tasks.forEach((task, index) => {
        const li = document.createElement("li");
        li.textContent = task.text;
        if(task.completed) li.classList.add("completed");

        // عند الضغط على المهمة لتعليمها كمكتملة
        li.addEventListener("click", () => {
          tasks[index].completed = !tasks[index].completed;
          localStorage.setItem("tasks", JSON.stringify(tasks));
          renderTasks();
        });

        // زر حذف لكل مهمة
        const deleteBtn = document.createElement("button");
        deleteBtn.textContent = "حذف";
        deleteBtn.addEventListener("click", (e) => {
          e.stopPropagation(); // منع تعليم المهمة عند الضغط على الحذف
          tasks.splice(index, 1);
          localStorage.setItem("tasks", JSON.stringify(tasks));
          renderTasks();
        });

        li.appendChild(deleteBtn);
        list.appendChild(li);
      });
    }

    // إضافة مهمة جديدة
    button.addEventListener("click", () => {
      const taskText = input.value.trim();
      if(taskText) {
        tasks.push({ text: taskText, completed: false });
        localStorage.setItem("tasks", JSON.stringify(tasks));
        renderTasks();
        input.value = "";
      }
    });

    // عرض المهام عند تحميل الصفحة
    renderTasks();
  </script>
</body>
</html>
