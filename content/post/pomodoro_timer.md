+++
author = "thounny"
title = "Pomodoro Timer App"
date = "2024-10-17"
description = "Building a Pomodoro timer"
tags = [
    "projects",
]
categories = [
    "projects",
]
+++

# Building My Pomodoro Timer

Creating my own Pomodoro timer has been a fulfilling project that allowed me to dive deep into JavaScript, HTML, and CSS. This project not only tested my coding skills but also pushed me to problem-solve and innovate. In this blog post, I’ll share the journey of building the timer, the errors I encountered along the way, and how the **Serenity Moment** timer inspired my design and functionality choices.

## Inspiration from Serenity Moment

I discovered the **Serenity Moment** website: https://serenitymoment.app/timer while scrolling through my LinkedIn feed. Designed and developed by [Quentin Hocdé](https://quentinhocde.com/), the clean layout, user-friendly interface, and elegant timer animations instantly captured my attention. I wanted to replicate the essence of their design while adding my unique touch. The timer on Serenity Moment stands out because of its simplicity and effectiveness, which is exactly what I aimed to achieve in my project.

### Key Features I Wanted to Incorporate:

- **A Minimalist Design**: I aimed for a clean aesthetic that allows users to focus on their tasks.
- **Responsive Layout**: The timer should work seamlessly on both desktop and mobile devices.
- **Interactive Elements**: Users should be able to easily adjust work and break durations, add tasks, and see their progress.

## Project Setup

### Technologies Used:

- **HTML**: For structuring the web page.
- **CSS**: To style the layout and give it a modern touch.
- **JavaScript**: To add interactivity, including the timer countdown and task management.

### Initial Development

The initial setup involved creating the HTML structure. I divided the layout into distinct sections:

- A header with navigation links.
- A main section for the Pomodoro timer.
- A task input area where users can add their tasks.

Here’s a simplified version of my HTML structure:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Wired Pomodoro</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <header>
      <nav>
        <div class="logo">Wired</div>
        <ul>
          <li><a href="#">Timer</a></li>
          <li><a href="#">Dashboard</a></li>
          <li><a href="#">Sessions</a></li>
        </ul>
      </nav>
    </header>
    <main>
      <section class="pomodoro-timer">
        <h2>POMODORO TIMER</h2>
        <div class="time-settings">...</div>
        <button id="start-session" class="minimal-btn">Start Session</button>
      </section>
      <section class="task-container">...</section>
    </main>
    <script src="app.js"></script>
  </body>
</html>
```

### Styling with CSS

I wanted my Pomodoro timer to have a visually appealing design, inspired by the Serenity Moment website. I focused on a dark background with light-colored text, using soft transitions and animations to enhance user interaction.

Here’s a snippet of my CSS:

```css
body {
  background-color: #111;
  color: #f5e0c3;
  font-family: "Helvetica Neue", sans-serif;
}

header {
  width: 100%;
  position: fixed;
  top: 0;
  padding: 20px;
  background-color: #111;
}

.pomodoro-timer {
  text-align: center;
  margin-bottom: 50px;
}
```

## Challenges Faced

While building this project, I encountered several challenges:

### 1. Timer Functionality

Implementing a working timer was initially complex. I needed to ensure the timer counted down accurately, switched between work and break sessions, and updated dynamically.

### 2. Task Management

Managing tasks proved tricky. I wanted to add a smooth strike-through animation when tasks were completed and ensure they faded out properly from the list. This required careful manipulation of the DOM and event listeners.

### 3. Responsive Design

Making the design responsive was another hurdle. Ensuring the layout looked good on both desktop and mobile screens required thoughtful CSS media queries.

## Solutions and Final Implementation

To address these issues:

- I used **setInterval** to create the countdown functionality for the timer and added logic to switch between work and break times.
- For task management, I implemented event listeners to handle adding tasks and marking them as completed with a strike-through effect using CSS transitions.
- I applied media queries to my CSS to ensure the layout was responsive across different screen sizes.

### Code Snippets for Key Functions

Here are snippets of key functions from my JavaScript:

```javascript
function startTimer() {
  // Timer logic here...
}

function addTask() {
  const taskInput = document.getElementById("task-input");
  // Add task logic here...
}
```

## JavaScript: The Heart of the Pomodoro Timer

JavaScript plays a crucial role in making the Pomodoro timer interactive and dynamic. In this section, I will break down the structure of the JavaScript code, explain the key functions, and illustrate how they contribute to the overall functionality of the project.

### Project Structure

The JavaScript code is organized to manage three primary functionalities:

1. **Timer Management**: Handling the countdown of work and break durations.
2. **Task Management**: Adding, checking off, and removing tasks.
3. **User Interaction**: Handling button clicks and updating the UI.

### Key Functions and Their Roles

Here are the essential functions that make the Pomodoro timer work:

#### 1. **Toggle Timer Functionality**

The `toggleTimer` function manages the state of the timer. It starts or stops the timer based on its current state.

```javascript
function toggleTimer() {
  const button = document.getElementById("start-session");
  if (isRunning) {
    stopTimer();
    button.innerText = "Start Session";
  } else {
    startTimer();
    button.innerText = "Stop Session";
  }
}
```

#### 2. **Starting the Timer**

The `startTimer` function initiates the countdown based on whether the user is in a work or break session.

```javascript
function startTimer() {
  let minutes = isBreak ? breakDuration : workDuration;
  let seconds = 0;

  isRunning = true;

  timerInterval = setInterval(() => {
    if (seconds === 0) {
      if (minutes === 0) {
        clearInterval(timerInterval);
        if (!isBreak) {
          showBreakIndicator();
          minutes = breakDuration;
          isBreak = true;
          startTimer(); // Start the break timer
        } else {
          isBreak = false;
          resetTimer(); // Reset to work timer after break
          hideBreakIndicator();
        }
      } else {
        minutes--;
        seconds = 59;
      }
    } else {
      seconds--;
    }

    updateTimerDisplay(minutes, seconds);
  }, 1000);
}
```

#### 3. **Stopping the Timer**

The `stopTimer` function stops the countdown and resets the timer back to the work duration:

```javascript
function stopTimer() {
  clearInterval(timerInterval);
  resetTimer();
  isRunning = false;
}
```

#### 4. **Updating the Timer Display**

The `updateTimerDisplay` function formats and displays the time left in the timer.

```javascript
function updateTimerDisplay(minutes, seconds) {
  document.getElementById("minutes").innerText = String(minutes).padStart(
    2,
    "0"
  );
  document.getElementById("seconds").innerText = String(seconds).padStart(
    2,
    "0"
  );
}
```

#### 5. **Resetting the Timer**

The `resetTimer` function sets the timer back to the initial work duration and hides the break indicator.

```javascript
function resetTimer() {
  document.getElementById("minutes").innerText = workDuration;
  document.getElementById("seconds").innerText = "00";
  isBreak = false;
  hideBreakIndicator(); // Ensure the break indicator is hidden when resetting
}
```

#### 6. **Task Management**

The `addTask` function allows users to add tasks to their task list, complete them, and remove them when done.

```javascript
function addTask() {
  const taskInput = document.getElementById("task-input");
  const taskList = document.getElementById("task-list");

  if (taskInput.value.trim() !== "") {
    const newTask = document.createElement("li");
    newTask.classList.add("task-item");
    newTask.innerHTML = `
      <label class="task-item">
        <input type="radio" class="task-check" />
        <span>${taskInput.value}</span>
      </label>
    `;

    newTask
      .querySelector(".task-check")
      .addEventListener("change", function () {
        const taskSpan = this.nextElementSibling;
        newTask.classList.add("completed"); // Add the completed class first
        setTimeout(() => {
          newTask.remove(); // Remove the task after fading out
        }, 500); // Match this timeout to the CSS transition duration
      });

    taskList.appendChild(newTask);
    taskInput.value = "";
  }
}
```

### Conclusion

Building my Pomodoro timer has been a rewarding experience that taught me valuable lessons in coding and problem-solving. The influence of the **Serenity Moment** website was instrumental in shaping my design and functionality choices. I encourage fellow developers to take inspiration from existing projects and adapt them to their unique vision.

This project not only improved my technical skills but also reinforced my understanding of user-centered design principles. I look forward to enhancing this project further and possibly integrating more features, such as analytics for tracking productivity.
