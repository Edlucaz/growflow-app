# Growflow: Your Daily Task Organizer

Welcome to **Growflow**, a simple and elegant daily task management application designed to help you stay organized and productive.  
This application provides a clean, user-friendly interface to manage your daily to-do lists, track progress, and highlight important tasks.

---

## ✨ Features

- **Intuitive Task Management:** Easily add, edit, and delete tasks with a straightforward interface.
- **Task Prioritization:** Categorize tasks by `Work`, `Personal`, or `Study` and assign priority levels (`High`, `Medium`, `Low`).
- **Date-Based Filtering:** Navigate through your daily tasks using a simple date picker.
- **Progress Tracking:** A dynamic "Total Active Tasks" counter and a visual progress widget help you monitor your daily achievements.
- **Alert System:** Visual alerts immediately notify you of upcoming important tasks and overdue tasks.
- **Theming:** A dark mode toggle allows you to switch between light and dark themes for a comfortable viewing experience.

---

## 🖼️ Screenshots

### Default Light Theme
![Screenshot of the Growflow application in light mode with an empty task list.](src/assets/placeholder_light_empty.png)

### Dark Theme with Tasks
![Screenshot of the Growflow application in dark mode with a populated task list.](src/assets/placeholder_dark_tasks.png)

### Alerts in Action
![Screenshot showing the "Important Task Alert" and "Overdue Important Task" alerts in the sidebar.](src/assets/placeholder_alerts.png)

---

## 🛠️ Technical Stack

- **Frontend:** [Vue.js](https://vuejs.org/) for the reactive user interface.  
- **Styling:** CSS for custom, component-based styling, including a robust theme system using CSS variables.  
- **Build Tool:** [Vite](https://vitejs.dev/) for a fast development experience.  

---

## 🚀 Getting Started

Follow these steps to set up and run the project locally.

### Prerequisites

- [Node.js](https://nodejs.org/) and npm (or `yarn`) installed on your machine.

### Installation

1. Navigate to the project directory:
   ```bash
   cd growflow-app
   ```

2. Install the dependencies:
   ```bash
   npm install
   ```

### Running the Application

To start the development server, run the following command:

```bash
npm run dev
```

The application will be available at [http://localhost:5173](http://localhost:5173).  
The server will automatically reload when you make changes to the code.

---

## 📂 Project Structure

- **src/main.js**: Initializes the Vue application.  
- **src/App.vue**: Primary Vue component containing the main template, script, and styling.  
- **src/style.css**: Custom CSS for styling components and managing themes.  

---

## 🎨 Customization

### Theming

The application uses **CSS variables** for theming, making it easy to create new themes or modify existing ones.  
Theme variables can be found at the top of `src/style.css`.

### Alerts

The alert boxes in the sidebar are styled using the classes:  
- `.important-task-alert`  
- `.overdue-important-alert`  

These components are designed to be visually distinct to catch the user's attention.

---

## 🤝 Contributing

We welcome contributions! 🚀  
If you have suggestions for new features, bug fixes, or improvements, feel free to open an issue or submit a pull request.
