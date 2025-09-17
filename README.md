Growflow: Your Daily Task Organizer

Welcome to Growflow, a simple and elegant daily task management application designed to help you stay organized and productive. This application provides a clean, user-friendly interface to manage your daily to-do lists, track progress, and highlight important tasks.

Features

    Intuitive Task Management: Easily add, edit, and delete tasks with a straightforward interface.

    Task Prioritization: Categorize tasks by Work, Personal, or Study and assign priority levels (High, Medium, Low).

    Date-Based Filtering: Navigate through your daily tasks using a simple date picker.

    Progress Tracking: A dynamic "Total Active Tasks" counter and a visual progress widget help you monitor your daily achievements.

    Alert System: Visual alerts immediately notify you of upcoming important tasks and overdue tasks.

    Theming: A dark mode toggle allows you to switch between light and dark themes for a comfortable viewing experience.

Screenshots

Default Light Theme

Dark Theme with Tasks

Alerts in Action

Technical Stack

    Frontend: Vue.js for the reactive user interface.

    Styling: CSS for custom, component-based styling, including a robust theme system using CSS variables.

    Build Tool: Vite for a fast development experience.

Getting Started

Follow these steps to set up and run the project locally.

Prerequisites

You need to have Node.js and npm (or yarn) installed on your machine.

Installation

    Navigate to the project directory:
    Bash

cd growflow

Install the dependencies:
Bash

    npm install

Running the Application

To start the development server, run the following command:
Bash

npm run dev

The application will be available at http://localhost:5173. The server will automatically reload when you make changes to the code.

Project Structure

    src/main.js: The main JavaScript file that initializes the Vue application.

    src/App.vue: The primary Vue component that structures the entire application, containing the main template, script, and styling.

    src/style.css: All the custom CSS for styling the components and managing themes.

Customization

Theming

The application uses CSS variables for theming, making it easy to create new themes or modify existing ones. You can find the theme variables at the top of src/style.css.

Alerts

The alert boxes in the sidebar are styled using the classes .important-task-alert and .overdue-important-alert. These components are designed to be visually distinct to catch the user's attention.

Contributing

We welcome contributions! If you have suggestions for new features, bug fixes, or improvements, feel free to open an issue or submit a pull request.
