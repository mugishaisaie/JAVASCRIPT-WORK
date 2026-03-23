# Assignment: Building Interactive Web Pages with JavaScript — Complete Solution Guide

> **Author:** Senior Developer Guide
> **Objective:** Understand how JavaScript interacts with webpages built using HTML and CSS by building small interactive features.

---

## Table of Contents

1. [Part 1: JavaScript Basics](#part-1-javascript-basics)
2. [Part 2: DOM Manipulation](#part-2-dom-manipulation)
3. [Part 3: Event Handling](#part-3-event-handling)
4. [Part 4: Build a Simple Calculator](#part-4-build-a-simple-calculator)
5. [Part 5: Mini Project – To-Do List](#part-5-mini-project--to-do-list)
6. [Evaluation Criteria Summary](#evaluation-criteria-summary)

---

---

## Part 1: JavaScript Basics

### What This Part Teaches

This part covers the **absolute fundamentals** of JavaScript:

- **Variable declaration** — storing data in named containers.
- **Template literals** — building strings that embed variable values.
- **Console output** — using `console.log()` to print messages to the browser's developer console.

### Key Concepts Explained

#### 1. Variables in JavaScript

A **variable** is a named container that stores a value. JavaScript provides three keywords to declare variables:

| Keyword | Scope        | Reassignable? | Hoisted? | Best Used For                        |
|---------|-------------|---------------|----------|--------------------------------------|
| `var`   | Function    | Yes           | Yes      | Legacy code (avoid in modern JS)     |
| `let`   | Block `{}`  | Yes           | No       | Values that will change              |
| `const` | Block `{}`  | No            | No       | Values that should never be reassigned |

**Why `const` by default?** In professional codebases, we use `const` unless we specifically need to reassign a variable. This prevents accidental overwrites and makes code more predictable.

#### 2. Template Literals (Backticks)

Template literals use backticks (`` ` ``) instead of quotes. They allow you to embed expressions directly inside a string using `${expression}` syntax:

```javascript
// Old way (string concatenation) — harder to read
const message = "Welcome " + name + " to the " + course + " course.";

// Modern way (template literals) — clean and readable
const message = `Welcome ${name} to the ${course} course.`;
```

#### 3. `console.log()`

`console.log()` prints output to the **browser developer console** (open with `F12` or `Ctrl+Shift+I`). This is the primary debugging tool for JavaScript developers.

### Complete Code Solution

#### `part1.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Part 1 - JavaScript Basics</title>
</head>
<body>
    <h1>Part 1: JavaScript Basics</h1>
    <p>Open the browser console (F12) to see the output.</p>

    <script>
        // -------------------------------------------------------
        // STEP 1: Declare variables
        // -------------------------------------------------------
        // We use 'const' because these values will not change
        // after being assigned.

        const studentName = "Alice";
        const courseName  = "Frontend Development";
        const year         = 2026;

        // -------------------------------------------------------
        // STEP 2: Display the welcome message in the console
        // -------------------------------------------------------
        // Template literals (backtick strings) let us embed
        // variables directly inside the string with ${}.

        console.log(`Welcome ${studentName} to the ${courseName} course.`);

        // BONUS: Also log the year
        console.log(`Academic Year: ${year}`);
    </script>
</body>
</html>
```

### Console Output

```
Welcome Alice to the Frontend Development course.
Academic Year: 2026
```

### Line-by-Line Explanation

| Line | What It Does |
|------|-------------|
| `const studentName = "Alice";` | Creates a constant variable called `studentName` and stores the text `"Alice"`. |
| `const courseName = "Frontend Development";` | Stores the course name string. |
| `const year = 2026;` | Stores a number (no quotes = number type, not a string). |
| `` console.log(`Welcome ${studentName}...`) `` | Builds a message by inserting variable values into the template string, then prints it to the console. |

### Why This Matters

- Variables are the **building blocks** of every program. All data flows through variables.
- `console.log()` is the **#1 debugging tool** — before using any fancy debugger, developers log values to check if their code works.
- Template literals are the **modern professional standard** for building strings.

---

---

## Part 2: DOM Manipulation

### What This Part Teaches

This part introduces the **Document Object Model (DOM)** — the programming interface that allows JavaScript to read, modify, add, and delete HTML elements on a live webpage.

### Key Concepts Explained

#### 1. What Is the DOM?

When a browser loads an HTML page, it creates a **tree-like structure** in memory called the DOM. Every HTML tag becomes a **node** in this tree:

```
document
  └── html
       ├── head
       │    └── title
       └── body
            ├── h1  ← "Welcome to my website"
            └── button ← "Change Heading"
```

JavaScript can reach into this tree and manipulate any node — that's the power of DOM manipulation.

#### 2. `document.getElementById()`

This method searches the entire DOM tree for an element whose `id` attribute matches the string you pass in:

```javascript
// HTML: <h1 id="heading">Hello</h1>
const heading = document.getElementById("heading");
// 'heading' now holds a reference to that <h1> element
```

**Why IDs?** An `id` must be unique on the page, so `getElementById()` always returns exactly one element (or `null` if not found).

#### 3. `.textContent` vs `.innerHTML`

| Property       | What It Does                            | Security  |
|----------------|----------------------------------------|-----------|
| `.textContent` | Gets/sets the plain text of an element | Safe      |
| `.innerHTML`   | Gets/sets the HTML content (parses tags) | Risky (XSS) |

**Best practice:** Use `.textContent` when you only need to change text. It's faster and immune to cross-site scripting (XSS) attacks.

#### 4. `onclick` Event Attribute

The `onclick` attribute on a button runs JavaScript code when the user clicks it. This is the simplest way to attach an event (Part 3 shows the professional way).

### Complete Code Solution

#### `part2.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Part 2 - DOM Manipulation</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 50px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 5px;
        }
        button:hover {
            background-color: #2980b9;
        }
    </style>
</head>
<body>

    <!-- The heading we will change -->
    <h1 id="heading">Welcome to my website</h1>

    <!-- The button that triggers the change -->
    <button onclick="changeHeading()">Change Heading</button>

    <script>
        // -------------------------------------------------------
        // Function: changeHeading
        // Purpose:  Finds the <h1> element by its ID and replaces
        //           its text content with a new message.
        // -------------------------------------------------------
        function changeHeading() {
            // STEP 1: Grab the element from the DOM
            // document.getElementById("heading") searches the
            // page for an element with id="heading" and returns it.
            const heading = document.getElementById("heading");

            // STEP 2: Change its text
            // .textContent replaces the visible text inside the element.
            heading.textContent = "JavaScript is controlling this page!";
        }
    </script>

</body>
</html>
```

### How It Works Step by Step

```
1. Page loads → browser reads HTML → builds the DOM tree
2. User sees: "Welcome to my website" and a [Change Heading] button
3. User clicks the button → onclick="changeHeading()" fires
4. JavaScript executes changeHeading():
     a. document.getElementById("heading") → finds the <h1>
     b. .textContent = "JavaScript is controlling this page!"
        → replaces the <h1>'s text in real time
5. User now sees: "JavaScript is controlling this page!"
```

### Before and After

| State          | Heading Text                               |
|----------------|--------------------------------------------|
| **Before** click | `Welcome to my website`                   |
| **After** click  | `JavaScript is controlling this page!`    |

### Why This Matters

- DOM manipulation is **the core of front-end development**. Every interactive website you've ever used changes the page through the DOM.
- `getElementById()` is the simplest selector — later you'll learn `querySelector()`, which is more powerful.

---

---

## Part 3: Event Handling

### What This Part Teaches

This part introduces **event listeners** — the professional, modern way to handle user interactions in JavaScript.

### Key Concepts Explained

#### 1. What Is an Event?

An **event** is anything the user does on the page: clicking, typing, scrolling, hovering, submitting a form, etc. JavaScript can **listen** for these events and run code in response.

Common events:

| Event       | Triggers When...                    |
|-------------|-------------------------------------|
| `click`     | User clicks an element              |
| `mouseover` | Mouse hovers over an element        |
| `keydown`   | User presses a key                  |
| `submit`    | A form is submitted                 |
| `load`      | The page finishes loading           |
| `input`     | User types in an input field        |

#### 2. `addEventListener()` vs `onclick`

| Feature             | `onclick` (Part 2)            | `addEventListener()` (Part 3)         |
|---------------------|-------------------------------|---------------------------------------|
| Syntax              | HTML attribute or JS property | JavaScript method                     |
| Multiple handlers   | Only 1 per element            | Unlimited per element                 |
| Separation of concerns | Mixes HTML and JS          | Keeps HTML and JS separate            |
| Removable           | Not easily                    | Yes, with `removeEventListener()`     |
| **Best practice?**  | No                            | **Yes** — this is the professional way |

#### 3. How `addEventListener()` Works

```javascript
element.addEventListener(eventType, callbackFunction);
```

- **`eventType`** — a string like `"click"`, `"mouseover"`, `"keydown"`
- **`callbackFunction`** — the function to execute when the event fires

The key insight: you are telling the browser *"when this event happens on this element, call this function."*

### Complete Code Solution

#### `part3.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Part 3 - Event Handling</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 50px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            background-color: #27ae60;
            color: white;
            border: none;
            border-radius: 5px;
        }
        button:hover {
            background-color: #219a52;
        }
        #message {
            font-size: 18px;
            margin-top: 20px;
            color: #2c3e50;
        }
    </style>
</head>
<body>

    <h1>Part 3: Event Handling</h1>

    <!-- Button: no onclick attribute — we attach the event in JS -->
    <button id="myButton">Click Me</button>

    <!-- Paragraph: starts empty, gets filled when button is clicked -->
    <p id="message"></p>

    <script>
        // -------------------------------------------------------
        // STEP 1: Get references to the DOM elements
        // -------------------------------------------------------
        const button  = document.getElementById("myButton");
        const message = document.getElementById("message");

        // -------------------------------------------------------
        // STEP 2: Attach an event listener
        // -------------------------------------------------------
        // .addEventListener("click", function) tells the browser:
        // "When the user clicks this button, run this function."
        //
        // Notice: we do NOT use onclick="" in the HTML.
        // This keeps our HTML clean and our logic in JavaScript.

        button.addEventListener("click", function () {
            // This code runs ONLY when the button is clicked
            message.textContent = "You clicked the button!";
        });
    </script>

</body>
</html>
```

### How It Works Step by Step

```
1. Page loads → JavaScript runs immediately:
     a. Gets a reference to the <button> (stored in 'button')
     b. Gets a reference to the <p> (stored in 'message')
     c. Attaches a click listener to the button
2. User sees: a "Click Me" button and an empty paragraph
3. User clicks the button → the "click" event fires
4. The callback function runs:
     → message.textContent = "You clicked the button!"
5. The paragraph now displays: "You clicked the button!"
```

### Why `addEventListener` Is Better Than `onclick`

```javascript
// With onclick: the second handler OVERWRITES the first
button.onclick = function () { console.log("First"); };
button.onclick = function () { console.log("Second"); }; // "First" is lost!

// With addEventListener: BOTH handlers run
button.addEventListener("click", function () { console.log("First"); });
button.addEventListener("click", function () { console.log("Second"); });
// Click → "First" then "Second" — both execute!
```

### Why This Matters

- `addEventListener()` is the **industry standard**. Every professional codebase, library (React, Vue, Angular), and framework uses this pattern internally.
- It enforces **separation of concerns** — HTML handles structure, JavaScript handles behavior.

---

---

## Part 4: Build a Simple Calculator

### What This Part Teaches

This part combines everything learned so far:

- **DOM manipulation** — reading input values and writing results to the page
- **Event handling** — responding to button clicks
- **JavaScript logic** — performing arithmetic operations
- **Type conversion** — converting strings to numbers with `parseFloat()`

### Key Concepts Explained

#### 1. Reading Input Values

HTML `<input>` elements store whatever the user types. You access the typed value with `.value`:

```javascript
const inputElement = document.getElementById("num1");
const userTyped = inputElement.value; // This is always a STRING like "10"
```

**Critical gotcha:** `.value` always returns a **string**, even if the user types a number. `"10" + "5"` gives `"105"` (string concatenation), not `15`. You must convert it.

#### 2. `parseFloat()` — Converting Strings to Numbers

| Function       | What It Does                          | Example                  |
|----------------|---------------------------------------|--------------------------|
| `parseFloat()` | Converts a string to a decimal number | `parseFloat("10.5")` → `10.5` |
| `parseInt()`   | Converts a string to a whole number   | `parseInt("10.9")` → `10`     |
| `Number()`     | Converts a string to a number         | `Number("10.5")` → `10.5`     |

We use `parseFloat()` here because it handles both whole numbers and decimals.

#### 3. Division by Zero

When you divide by zero in JavaScript, you get `Infinity` (not an error). Professional code checks for this:

```javascript
if (num2 === 0) {
    result = "Cannot divide by zero";
} else {
    result = num1 / num2;
}
```

### Complete Code Solution

#### `part4.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Part 4 - Simple Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 40px;
            background-color: #f5f5f5;
        }
        .calculator {
            background: white;
            max-width: 400px;
            margin: 0 auto;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        input {
            padding: 10px;
            font-size: 16px;
            width: 80%;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        button {
            padding: 12px 30px;
            font-size: 16px;
            cursor: pointer;
            background-color: #e67e22;
            color: white;
            border: none;
            border-radius: 5px;
            margin-top: 10px;
        }
        button:hover {
            background-color: #d35400;
        }
        #results {
            margin-top: 20px;
            text-align: left;
            padding: 15px;
            background: #ecf0f1;
            border-radius: 5px;
            font-size: 16px;
            line-height: 1.8;
        }
    </style>
</head>
<body>

    <div class="calculator">
        <h1>Simple Calculator</h1>

        <!-- Input fields for two numbers -->
        <input type="number" id="num1" placeholder="Enter first number"><br>
        <input type="number" id="num2" placeholder="Enter second number"><br>

        <!-- Button to trigger the calculation -->
        <button id="calcBtn">Calculate</button>

        <!-- Results area (starts empty) -->
        <div id="results"></div>
    </div>

    <script>
        // -------------------------------------------------------
        // Get references to DOM elements
        // -------------------------------------------------------
        const calcBtn    = document.getElementById("calcBtn");
        const resultsDiv = document.getElementById("results");

        // -------------------------------------------------------
        // Attach event listener to the Calculate button
        // -------------------------------------------------------
        calcBtn.addEventListener("click", function () {

            // STEP 1: Read the values the user typed
            // .value returns a STRING, so we must convert to numbers
            const num1 = parseFloat(document.getElementById("num1").value);
            const num2 = parseFloat(document.getElementById("num2").value);

            // STEP 2: Validate — check if user actually entered numbers
            // isNaN() returns true if the value is Not a Number
            if (isNaN(num1) || isNaN(num2)) {
                resultsDiv.textContent = "Please enter valid numbers in both fields.";
                return; // Stop the function here — don't continue
            }

            // STEP 3: Perform arithmetic operations
            const addition       = num1 + num2;
            const subtraction    = num1 - num2;
            const multiplication = num1 * num2;

            // Handle division by zero
            const division = (num2 !== 0) ? (num1 / num2) : "Cannot divide by zero";

            // STEP 4: Display the results on the page
            // We use innerHTML here because we need <br> line breaks
            resultsDiv.innerHTML = `
                <strong>Number 1:</strong> ${num1}<br>
                <strong>Number 2:</strong> ${num2}<br><br>
                <strong>Addition:</strong> ${num1} + ${num2} = ${addition}<br>
                <strong>Subtraction:</strong> ${num1} - ${num2} = ${subtraction}<br>
                <strong>Multiplication:</strong> ${num1} × ${num2} = ${multiplication}<br>
                <strong>Division:</strong> ${num1} ÷ ${num2} = ${division}
            `;
        });
    </script>

</body>
</html>
```

### How It Works Step by Step

```
1. Page loads → JS gets references to elements && attaches click listener
2. User types: 10 into first input, 5 into second input
3. User clicks "Calculate" → event fires → callback runs:
     a. parseFloat("10") → 10   (string to number)
     b. parseFloat("5")  → 5    (string to number)
     c. isNaN(10) → false, isNaN(5) → false → validation passes
     d. addition       = 10 + 5  = 15
     e. subtraction    = 10 - 5  = 5
     f. multiplication = 10 * 5  = 50
     g. division       = 10 / 5  = 2
     h. Results are inserted into the page via innerHTML
4. User sees all four results displayed
```

### Example Output

```
Number 1: 10
Number 2: 5

Addition:       10 + 5 = 15
Subtraction:    10 - 5 = 5
Multiplication: 10 × 5 = 50
Division:       10 ÷ 5 = 2
```

### Important Details

| Concept | Explanation |
|---------|------------|
| `parseFloat()` | Converts `"10"` (string) to `10` (number). Without this, `"10" + "5"` = `"105"` |
| `isNaN()` | Returns `true` if the value is not a valid number. Used for input validation. |
| `return` | Exits the function early if validation fails, preventing further calculations. |
| Ternary `? :` | Short `if/else`: `(condition) ? valueIfTrue : valueIfFalse` |

---

---

## Part 5: Mini Project – To-Do List

### What This Part Teaches

This is the **capstone project** that combines everything:

- Variable declaration
- DOM manipulation (creating, reading, modifying, removing elements)
- Event handling (`addEventListener`)
- Working with dynamic lists of elements
- Professional code organization

### Key Concepts Explained

#### 1. `document.createElement()`

You can create **brand new HTML elements** in JavaScript that don't exist in the original HTML:

```javascript
const li = document.createElement("li");  // Creates a <li> element in memory
li.textContent = "Study JavaScript";      // Sets its text
document.getElementById("list").appendChild(li);  // Adds it to the page
```

#### 2. `.appendChild()` and `.removeChild()`

| Method            | What It Does                                    |
|-------------------|-------------------------------------------------|
| `.appendChild()`  | Adds a child element at the end of a parent     |
| `.removeChild()`  | Removes a specific child from a parent          |
| `.remove()`       | Removes the element itself (modern shorthand)   |

#### 3. `.trim()`

Removes whitespace from both ends of a string. Used to prevent users from adding empty tasks:

```javascript
"  hello  ".trim()  // → "hello"
"          ".trim()  // → "" (empty — no real content)
```

#### 4. `.classList.toggle()`

Adds a CSS class if it's absent, removes it if it's present. Perfect for toggling "completed" state:

```javascript
element.classList.toggle("completed");
// First call:  adds "completed" class    → text gets strikethrough
// Second call: removes "completed" class → text returns to normal
```

### Complete Code Solution

#### `part5.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Part 5 - To-Do List</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 40px;
        }

        .todo-container {
            max-width: 500px;
            margin: 0 auto;
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 2px 15px rgba(0,0,0,0.1);
        }

        h1 {
            text-align: center;
            color: #2c3e50;
        }

        /* Input area: input field + add button side by side */
        .input-area {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }

        .input-area input {
            flex: 1;          /* Take up all remaining space */
            padding: 10px;
            font-size: 16px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        .input-area button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #27ae60;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .input-area button:hover {
            background-color: #219a52;
        }

        /* The task list */
        ul {
            list-style: none;    /* Remove bullet points */
            padding: 0;
        }

        ul li {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 15px;
            margin-bottom: 8px;
            background-color: #ecf0f1;
            border-radius: 5px;
            font-size: 16px;
        }

        /* Completed task style — strikethrough + gray */
        ul li.completed {
            text-decoration: line-through;
            color: #95a5a6;
            background-color: #d5dbdb;
        }

        /* Task text (clickable to toggle complete) */
        .task-text {
            cursor: pointer;
            flex: 1;
        }

        /* Delete button on each task */
        .delete-btn {
            background-color: #e74c3c;
            color: white;
            border: none;
            padding: 5px 12px;
            border-radius: 3px;
            cursor: pointer;
            font-size: 14px;
            margin-left: 10px;
        }

        .delete-btn:hover {
            background-color: #c0392b;
        }
    </style>
</head>
<body>

    <div class="todo-container">
        <h1>To-Do List</h1>

        <!-- Input area: text field + Add button -->
        <div class="input-area">
            <input type="text" id="taskInput" placeholder="Enter a new task...">
            <button id="addBtn">Add</button>
        </div>

        <!-- Task list (starts empty — tasks are added dynamically) -->
        <ul id="taskList"></ul>
    </div>

    <script>
        // -------------------------------------------------------
        // STEP 1: Get references to DOM elements
        // -------------------------------------------------------
        const taskInput = document.getElementById("taskInput");
        const addBtn    = document.getElementById("addBtn");
        const taskList  = document.getElementById("taskList");

        // -------------------------------------------------------
        // STEP 2: Function to add a new task
        // -------------------------------------------------------
        function addTask() {
            // Read and trim the input value
            const taskText = taskInput.value.trim();

            // Validate: don't allow empty tasks
            if (taskText === "") {
                alert("Please enter a task!");
                return;
            }

            // CREATE the <li> element
            const li = document.createElement("li");

            // CREATE a <span> for the task text
            // (Clickable — clicking it toggles the "completed" state)
            const span = document.createElement("span");
            span.textContent = taskText;
            span.className = "task-text";

            // BONUS: Click the task text to mark it as completed
            span.addEventListener("click", function () {
                li.classList.toggle("completed");
            });

            // CREATE a delete button for this task
            const deleteBtn = document.createElement("button");
            deleteBtn.textContent = "Remove";
            deleteBtn.className = "delete-btn";

            // When the delete button is clicked, remove the <li>
            deleteBtn.addEventListener("click", function () {
                taskList.removeChild(li);
            });

            // ASSEMBLE: put the span and button inside the <li>
            li.appendChild(span);
            li.appendChild(deleteBtn);

            // ADD the <li> to the <ul> (display it on the page)
            taskList.appendChild(li);

            // CLEAR the input field for the next task
            taskInput.value = "";

            // FOCUS the input so the user can keep typing
            taskInput.focus();
        }

        // -------------------------------------------------------
        // STEP 3: Attach event listeners
        // -------------------------------------------------------

        // Click the "Add" button → add the task
        addBtn.addEventListener("click", addTask);

        // Press "Enter" key inside the input → add the task
        taskInput.addEventListener("keypress", function (event) {
            if (event.key === "Enter") {
                addTask();
            }
        });
    </script>

</body>
</html>
```

### How It Works — Full Walkthrough

```
ADDING A TASK:
═════════════
1. User types "Study JavaScript" in the input field
2. User clicks "Add" (or presses Enter)
3. addTask() runs:
     a. Reads input: "Study JavaScript"
     b. .trim() removes extra whitespace
     c. Validation: not empty → continue
     d. Creates <li> element
     e. Creates <span> with text "Study JavaScript"
     f. Attaches click listener to <span> (for marking complete)
     g. Creates <button> "Remove"
     h. Attaches click listener to <button> (for deleting)
     i. Assembles: <li> ← <span> + <button>
     j. Appends <li> to <ul>
     k. Clears the input field

RESULTING HTML (generated by JavaScript):
═════════════════════════════════════════
<ul id="taskList">
    <li>
        <span class="task-text">Study JavaScript</span>
        <button class="delete-btn">Remove</button>
    </li>
</ul>

MARKING A TASK COMPLETE (Bonus feature):
═════════════════════════════════════════
1. User clicks the task text "Study JavaScript"
2. classList.toggle("completed") adds the class
3. CSS applies: line-through + gray color
4. Click again → class is removed → text returns to normal

REMOVING A TASK:
════════════════
1. User clicks "Remove" button next to a task
2. taskList.removeChild(li) removes the <li> from the DOM
3. The task disappears from the page
```

### DOM Tree Visualization

After adding three tasks, the DOM looks like this:

```
<ul id="taskList">
  ├── <li>
  │    ├── <span class="task-text">Study JavaScript</span>
  │    └── <button class="delete-btn">Remove</button>
  ├── <li>
  │    ├── <span class="task-text">Finish assignment</span>
  │    └── <button class="delete-btn">Remove</button>
  └── <li class="completed">                              ← marked complete
       ├── <span class="task-text">Practice coding</span> ← strikethrough
       └── <button class="delete-btn">Remove</button>
```

### Why This Matters

This mini-project is a **real-world application pattern**. The exact same principles (create elements, attach events, remove from DOM) are used in:

- Social media feeds (adding/removing posts)
- Shopping carts (adding/removing items)
- Chat applications (adding new messages)
- Email clients (marking as read, deleting)

---

---

## Evaluation Criteria Summary

| Criteria                      | Marks | Where It's Demonstrated        |
|-------------------------------|-------|--------------------------------|
| Correct JavaScript logic      | 30    | Parts 1, 4 (variables, math, validation) |
| DOM manipulation              | 20    | Parts 2, 5 (getElementById, createElement, textContent) |
| Event handling                | 20    | Parts 3, 4, 5 (addEventListener, click, keypress) |
| Mini project functionality    | 20    | Part 5 (add, display, remove, mark complete) |
| Code organization             | 10    | All parts (clean structure, comments, naming) |
| **Total**                     | **100** |                              |

---

## Quick Reference Cheat Sheet

### Variable Declaration
```javascript
const name = "Alice";    // Cannot be reassigned (preferred)
let count = 0;           // Can be reassigned
var old = "avoid this";  // Function-scoped (legacy)
```

### DOM Selection
```javascript
document.getElementById("id")        // Find by ID — returns 1 element
document.querySelector(".class")     // Find by CSS selector — returns 1st match
document.querySelectorAll("li")      // Find all matches — returns NodeList
```

### DOM Modification
```javascript
element.textContent = "New text";          // Change text (safe)
element.innerHTML = "<b>Bold</b>";         // Change HTML (careful with user input)
element.style.color = "red";               // Change inline CSS
element.classList.add("active");           // Add CSS class
element.classList.remove("active");        // Remove CSS class
element.classList.toggle("active");        // Toggle CSS class
```

### Creating & Removing Elements
```javascript
const el = document.createElement("div");  // Create new element
parent.appendChild(el);                    // Add to page
parent.removeChild(el);                    // Remove from page
```

### Event Handling
```javascript
element.addEventListener("click", function () {
    // Code runs when element is clicked
});

// With Enter key detection
input.addEventListener("keypress", function (event) {
    if (event.key === "Enter") {
        // Code runs when Enter is pressed
    }
});
```

### Type Conversion
```javascript
parseFloat("10.5")   // → 10.5 (string to decimal number)
parseInt("10")       // → 10   (string to integer)
String(42)           // → "42" (number to string)
isNaN(value)         // → true if value is not a number
```

---

> **Final Tip:** Open every HTML file in a browser and experiment! Change values, break things, fix them. The best way to learn JavaScript is by doing.
