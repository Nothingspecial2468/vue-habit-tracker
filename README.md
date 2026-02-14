# Habit Tracker App
  A simple habit tracking appliaction built with Vue 3 using component-based architecture.

## 🚀 Features

- Add new habits
- Delete habits
- Mark habits as completed
- Automatic streak calculation
- Consistency categorization
- Real-time statistics dashboard
- Visual streak progress bar
- Data persistence using localStorage

---

## 🧠 Concepts Practiced

- Component-based architecture
- Single Responsibility Principle
- Props (data down)
- Emits (events up)
- Reactive state and computed properties
- Clean separation of logic and presentation
- Local storage integration

---

## 🏗️ Project Structure

src/
|
|-- App.vue # Main state & logic
|
|-- components/
| |--HabitInput.vue # Handles user input
| |--HabitItem.vue # Single habit item
| |--HabitStats.vue # Displays computed statistics
| |--StreakBar.vue # Visual progress bar


---

## 🔁 Data Flow

- State is managed in **App.vue**
- Data flows down via **props**
- User actions flow up via **emits**
- UI updates reactively using computed properties

---

## 💾 Persistence

All habit data is stored in the browser using `localStorage`, ensuring the data remains after page refresh.

---

## 📌 Version

**v1.0 – Stable and Refactored**

Core functionality complete.  
Application structured into clean, responsibility-based components.

---

## 🎯 Purpose

This project was built to move from basic feature implementation to understanding structured component design in Vue.
