 let tasks = [];
    let currentFilter = 'all';
    let editingTaskId = null;

    document.addEventListener('DOMContentLoaded', function () {
      loadTasks();
      renderTasks();
      updateStats();
      document.getElementById('taskDate').valueAsDate = new Date();
    });

    function addTask() {
      const title = document.getElementById('taskTitle').value.trim();
      const description = document.getElementById('taskDescription').value.trim();
      const date = document.getElementById('taskDate').value;
      const time = document.getElementById('taskTime').value;
      const priority = document.getElementById('taskPriority').value;

      if (!title) {
        alert('Please enter a task title!');
        return;
      }

      const task = {
        id: Date.now(),
        title,
        description,
        date,
        time,
        priority,
        completed: false,
      };

      tasks.push(task);
      saveTasks();
      renderTasks();
      updateStats();
      resetForm();
    }

    function resetForm() {
      document.getElementById('taskTitle').value = '';
      document.getElementById('taskDescription').value = '';
      document.getElementById('taskDate').valueAsDate = new Date();
      document.getElementById('taskTime').value = '';
      document.getElementById('taskPriority').value = 'medium';
    }

    function filterTasks(filter, e) {
      currentFilter = filter;
      document.querySelectorAll('.filter-btn').forEach(btn => btn.classList.remove('active'));
      if (e) e.target.classList.add('active');
      renderTasks();
    }

    function getFilteredTasks() {
      const now = new Date();
      const todayStr = now.toDateString();
      switch (currentFilter) {
        case 'completed':
          return tasks.filter(t => t.completed);
        case 'pending':
          return tasks.filter(t => !t.completed);
        case 'overdue':
          return tasks.filter(t => !t.completed && t.date && new Date(t.date + ' ' + (t.time || '00:00')) < now);
        case 'today':
          return tasks.filter(t => t.date && new Date(t.date).toDateString() === todayStr);
        case 'high':
          return tasks.filter(t => t.priority === 'high');
        default:
          return tasks;
      }
    }

    function renderTasks() {
      const container = document.getElementById('tasksContainer');
      const filtered = getFilteredTasks();

      if (filtered.length === 0) {
        container.innerHTML = `
          <div class="empty-state" style="text-align:center;padding:60px 20px;color:#1e40af">
            <h3>No tasks found!</h3>
            <p>${currentFilter === 'all' ? 'Add your first task above to get started.' : 'No tasks match the current filter.'}</p>
          </div>`;
        return;
      }

      const priorityLabels = {
        low: '🟢 Low',
        medium: '🟡 Medium',
        high: '🔴 High',
      };

      container.innerHTML = filtered
        .sort((a, b) => (a.completed - b.completed || new Date(a.date) - new Date(b.date)))
        .map(task => {
          const overdue = !task.completed && task.date && new Date(task.date + ' ' + (task.time || '00:00')) < new Date();
          return `
            <div class="task-item" style="margin:20px;background:#f9fafb;border-left:5px solid ${
              task.completed ? '#22c55e' : overdue ? '#ef4444' : '#3b82f6'
            }">
              <div class="task-header" style="display:flex;justify-content:space-between;align-items:start;margin-bottom:10px;">
                <div>
                  <div class="task-title" style="font-weight:600;font-size:18px;color:#0f172a;${
                    task.completed ? 'text-decoration:line-through;color:#94a3b8;' : ''
                  }">${task.title}</div>
                  <div class="task-description" style="color:#475569;margin-top:5px">${task.description || ''}</div>
                </div>
                <span style="font-size:13px;text-transform:uppercase;font-weight:bold;color:#2563eb">${priorityLabels[task.priority]}</span>
              </div>
              <div class="task-meta" style="color:#64748b;font-size:14px;margin-bottom:10px;">
                ${task.date ? `📅 ${task.date}` : ''} ${task.time ? `⏰ ${task.time}` : ''}
                ${overdue ? ` <span style="color:red;">⚠️ Overdue</span>` : ''}
              </div>
              <div class="task-actions" style="display:flex;gap:10px;">
                <button class="btn" style="padding:5px 15px;font-size:14px" onclick="toggleComplete(${task.id})">
                  ${task.completed ? 'Undo' : 'Complete'}
                </button>
                <button class="btn" style="padding:5px 15px;font-size:14px;background:#facc15;color:#1f2937" onclick="editTask(${task.id})">
                  Edit
                </button>
                <button class="btn" style="padding:5px 15px;font-size:14px;background:#ef4444" onclick="deleteTask(${task.id})">
                  Delete
                </button>
              </div>
            </div>
          `;
        })
        .join('');
    }

    function toggleComplete(id) {
      const task = tasks.find(t => t.id === id);
      if (task) {
        task.completed = !task.completed;
        saveTasks();
        renderTasks();
        updateStats();
      }
    }

    function deleteTask(id) {
      if (confirm('Delete this task?')) {
        tasks = tasks.filter(t => t.id !== id);
        saveTasks();
        renderTasks();
        updateStats();
      }
    }

    function editTask(id) {
      const task = tasks.find(t => t.id === id);
      if (!task) return;
      editingTaskId = id;
      document.getElementById('editTaskTitle').value = task.title;
      document.getElementById('editTaskDescription').value = task.description;
      document.getElementById('editTaskDate').value = task.date;
      document.getElementById('editTaskTime').value = task.time;
      document.getElementById('editTaskPriority').value = task.priority;
      document.getElementById('editModal').style.display = 'block';
    }

    function saveEditTask() {
      const task = tasks.find(t => t.id === editingTaskId);
      if (!task) return;
      task.title = document.getElementById('editTaskTitle').value;
      task.description = document.getElementById('editTaskDescription').value;
      task.date = document.getElementById('editTaskDate').value;
      task.time = document.getElementById('editTaskTime').value;
      task.priority = document.getElementById('editTaskPriority').value;
      saveTasks();
      renderTasks();
      updateStats();
      closeEditModal();
    }

    function closeEditModal() {
      document.getElementById('editModal').style.display = 'none';
      editingTaskId = null;
    }

    function updateStats() {
      document.getElementById('totalTasks').textContent = tasks.length;
      document.getElementById('pendingTasks').textContent = tasks.filter(t => !t.completed).length;
      document.getElementById('completedTasks').textContent = tasks.filter(t => t.completed).length;
      document.getElementById('overdueTasks').textContent = tasks.filter(t => {
        return !t.completed && t.date && new Date(t.date + ' ' + (t.time || '00:00')) < new Date();
      }).length;
    }

    function saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(tasks));
    }

    function loadTasks() {
      tasks = JSON.parse(localStorage.getItem('tasks')) || [];
    }

    window.onclick = function (e) {
      if (e.target === document.getElementById('editModal')) {
        closeEditModal();
      }
    };
