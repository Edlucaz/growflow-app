<template>
  <div class="app-container">
    <div class="app-sidebar">
      <div class="sidebar-header">
        <h2>Growflow</h2>
        <button class="theme-btn" @click="toggleTheme">
          <span v-if="theme === 'light'">🌙</span>
          <span v-else>☀️</span>
        </button>
      </div>

      <div class="date-navigation sidebar-section">
        <button @click="changeDate(-1)">&#9664;</button>
        <input type="date" v-model="currentDate" />
        <button @click="changeDate(1)">&#9654;</button>
      </div>

      <div class="progress-widget sidebar-section">
        <span class="plant-icon">{{ plantStageIcon }}</span>
        <p class="progress-message">{{ plantProgressMessage }}</p>
      </div>

      <div class="total-tasks-widget sidebar-section">
        <h3>Total Active Tasks</h3>
        <p class="total-tasks-count">{{ totalActiveTasks }}</p>
      </div>

      <div class="category-tasks-widget sidebar-section">
        <h3>Tasks by Category</h3>
        <ul>
          <li>Work: {{ tasksByCategory.Work }}</li>
          <li>Personal: {{ tasksByCategory.Personal }}</li>
          <li>Study: {{ tasksByCategory.Study }}</li>
        </ul>
      </div>
      
      <div class="alerts-container">
        <div class="important-task-alert" v-if="nextImportantFutureTask" @click="goToTaskDate(nextImportantFutureTask)">
          <h4>Important Task Alert!</h4>
          <p>{{ nextImportantFutureTask.text }}</p>
          <small>Due on: {{ nextImportantFutureTask.creationDate }}</small>
        </div>
        
        <div class="overdue-important-alert" v-if="nextImportantOverdueTask" @click="goToTaskDate(nextImportantOverdueTask)">
          <h4>Overdue Important Task!</h4>
          <p>{{ nextImportantOverdueTask.text }}</p>
          <small>Created on: {{ nextImportantOverdueTask.creationDate }}</small>
        </div>
      </div>

    </div>

    <div class="app-main-content">
      <div class="main-header">
        <h1>My Daily Tasks</h1>
      </div>

      <input
        v-model="searchQuery"
        placeholder="Search tasks"
        class="search-input"
      />

      <div class="add-task">
        <input
          ref="newTaskInput"
          v-model="newTask.text"
          @keyup.enter="addTask"
          placeholder="Enter a new task"
        />
        <select v-model="newTask.category" class="category-select">
          <option value="">Category</option>
          <option value="Work">Work</option>
          <option value="Personal">Personal</option>
          <option value="Study">Study</option>
        </select>
        <select v-model="newTask.priority" class="priority-select">
          <option value="">Priority</option>
          <option value="Low">Low</option>
          <option value="Medium">Medium</option>
          <option value="High">High</option>
        </select>
        <button @click="addTask">Add</button>
      </div>
      
      <transition name="slide-fade" mode="out-in">
        <div class="task-list-container" :key="currentDate">
          <div v-if="filteredTasks.length > 0">
            <ul class="task-list">
              <draggable v-model="filteredTasks" item-key="id" @change="saveTasks" :disabled="isDraggingDisabled">
                <template #item="{ element }">
                  <li :class="['priority-' + element.priority.toLowerCase(), { 'done': element.done, 'overdue': isOverdue(element) }]">
                    <div class="task-card">
                      <input type="checkbox" v-model="element.done" @change="saveTasks" />
                      <div class="task-details">
                        <span>{{ element.text }}</span>
                        <span class="task-metadata">{{ element.category }} - {{ element.priority }}</span>
                      </div>
                      <button class="delete" @click="removeTask(element.id)">❌</button>
                    </div>
                  </li>
                </template>
              </draggable>
            </ul>
          </div>
          <div v-else class="empty-state">
            <span class="empty-state-icon">📝</span>
            <p>You have no tasks for this day. How about adding one?</p>
          </div>
        </div>
      </transition>
      
      <div class="app-footer">
        <p>&copy; 2025 - Growflow. All rights reserved.</p>
      </div>

    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import "./style.css";

export default {
  components: { draggable },
  data() {
    let tasks = {};
    try {
      tasks = JSON.parse(localStorage.getItem("tasks")) || {};
    } catch (e) {
      console.error("Failed to parse tasks from localStorage", e);
      tasks = {};
    }

    const initialDate = this.getFormattedDate(new Date());

    return {
      newTask: { text: "", category: "", priority: "" },
      searchQuery: "",
      tasks: tasks,
      theme: localStorage.getItem("theme") || "light",
      currentDate: initialDate,
      images: {
        seed: "🌱",
        sprout: "🌿",
        smallPlant: "🪴",
        bigPlant: "🌳",
        complete: "✅",
      },
    };
  },
  computed: {
    tasksForCurrentDate: {
      get() {
        return this.tasks[this.currentDate] || [];
      },
      set(newTasks) {
        this.tasks[this.currentDate] = newTasks;
      }
    },
    filteredTasks: {
      get() {
        const tasks = this.tasksForCurrentDate;
        if (!this.searchQuery) {
          return tasks;
        }
        return tasks.filter(task =>
          task.text.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      },
      set(newFilteredTasks) {
        // Se a busca estiver ativa, a lista filtrada é a nova lista
        // Caso contrário, a lista completa é a nova lista
        if (this.searchQuery) {
          // Aqui, você precisaria de uma lógica mais complexa para atualizar a lista original
          // No momento, a busca impede o arrastar, então essa lógica não é necessária, mas é uma boa prática
        } else {
          this.tasksForCurrentDate = newFilteredTasks;
        }
      }
    },
    isDraggingDisabled() {
      return !!this.searchQuery;
    },
    doneTasksCount() {
      return this.tasksForCurrentDate.filter(task => task.done).length;
    },
    totalTasksCount() {
      return this.tasksForCurrentDate.length;
    },
    plantStageIcon() {
      if (this.totalTasksCount === 0) return this.images.seed;
      const progress = this.doneTasksCount / this.totalTasksCount;
      if (progress === 0) return this.images.seed;
      if (progress < 0.3) return this.images.sprout;
      if (progress < 0.7) return this.images.smallPlant;
      if (progress < 1) return this.images.bigPlant;
      return this.images.complete;
    },
    plantProgressMessage() {
      if (this.totalTasksCount === 0) {
        return "Add your first task!";
      }
      if (this.doneTasksCount === this.totalTasksCount) {
        return "All tasks completed. Good job!";
      }
      return `${this.doneTasksCount} of ${this.totalTasksCount} tasks completed.`;
    },
    totalActiveTasks() {
      let count = 0;
      for (const date in this.tasks) {
        if (this.tasks[date] && Array.isArray(this.tasks[date])) {
          count += this.tasks[date].filter(task => !task.done).length;
        }
      }
      return count;
    },
    tasksByCategory() {
      const categories = { Work: 0, Personal: 0, Study: 0 };
      for (const date in this.tasks) {
        if (this.tasks[date] && Array.isArray(this.tasks[date])) {
          this.tasks[date].forEach(task => {
            if (categories.hasOwnProperty(task.category)) {
              categories[task.category]++;
            }
          });
        }
      }
      return categories;
    },
    nextImportantFutureTask() {
      let allTasks = [];
      for (const date in this.tasks) {
        if (this.tasks[date] && Array.isArray(this.tasks[date])) {
          allTasks = allTasks.concat(this.tasks[date]);
        }
      }

      const futureHighPriorityTasks = allTasks.filter(task => 
        task.priority === 'High' && !task.done && new Date(task.creationDate) > new Date(this.getFormattedDate(new Date()))
      );
      
      if (futureHighPriorityTasks.length === 0) {
        return null;
      }

      futureHighPriorityTasks.sort((a, b) => new Date(a.creationDate) - new Date(b.creationDate));

      return futureHighPriorityTasks[0];
    },
    nextImportantOverdueTask() {
      let allTasks = [];
      for (const date in this.tasks) {
        if (this.tasks[date] && Array.isArray(this.tasks[date])) {
          allTasks = allTasks.concat(this.tasks[date]);
        }
      }

      const overdueHighPriorityTasks = allTasks.filter(task => 
        task.priority === 'High' && !task.done && new Date(task.creationDate) < new Date(this.getFormattedDate(new Date()))
      );
      
      if (overdueHighPriorityTasks.length === 0) {
        return null;
      }

      overdueHighPriorityTasks.sort((a, b) => new Date(a.creationDate) - new Date(b.creationDate));

      return overdueHighPriorityTasks[0];
    }
  },
  methods: {
    getFormattedDate(date) {
      const year = date.getFullYear();
      const month = (date.getMonth() + 1).toString().padStart(2, '0');
      const day = date.getDate().toString().padStart(2, '0');
      return `${year}-${month}-${day}`;
    },
    changeDate(offset) {
      const newDate = new Date(this.currentDate + 'T00:00:00');
      newDate.setDate(newDate.getDate() + offset);
      this.currentDate = this.getFormattedDate(newDate);
    },
    addTask() {
      if (this.newTask.text.trim()) {
        const tasksForDay = [...this.tasksForCurrentDate];
        tasksForDay.push({
          id: Date.now(),
          text: this.newTask.text,
          category: this.newTask.category || "General",
          priority: this.newTask.priority || "Low",
          done: false,
          creationDate: this.currentDate,
        });
        this.tasksForCurrentDate = tasksForDay;
        this.newTask = { text: "", category: "", priority: "" };
        this.$nextTick(() => {
          this.$refs.newTaskInput.focus();
        });
      }
    },
    removeTask(taskId) {
      this.tasksForCurrentDate = this.tasksForCurrentDate.filter(task => task.id !== taskId);
    },
    saveTasks() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    toggleTheme() {
      this.theme = this.theme === "light" ? "dark" : "light";
      localStorage.setItem("theme", this.theme);
      document.body.className = this.theme;
    },
    isOverdue(task) {
      const today = this.getFormattedDate(new Date());
      return !task.done && task.creationDate < today;
    },
    goToTaskDate(task) {
        if (task && task.creationDate) {
            this.currentDate = task.creationDate;
        }
    }
  },
  mounted() {
    document.body.className = this.theme;
    this.$nextTick(() => {
      this.$refs.newTaskInput.focus();
    });
  },
  watch: {
    currentDate: {
      handler(newDate) {
        if (!this.tasks[newDate]) {
          this.tasks[newDate] = [];
        }
      },
      immediate: true,
    },
    tasks: {
      handler() {
        this.saveTasks();
      },
      deep: true,
    },
  },
};
</script>