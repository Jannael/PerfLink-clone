# PerfLink-clone

PerfLink-clone is a web-based JavaScript benchmarking tool designed to efficiently measure and compare the performance of code snippets. It allows you to define global variables and create multiple test cases to analyze which approach is faster.

## Features

*   **Precise Benchmarking**: Performance tests execution using `Web Workers` to keep the interface smooth and isolate execution.
*   **Code Editor**: Text areas with syntax highlighting to facilitate writing JavaScript code.
*   **Global Context**: Ability to define "Global" code that runs once before all test cases (ideal for initializing arrays, large objects, etc.).
*   **Local Persistence (IndexedDB)**: Benchmarks are automatically saved in the browser using IndexedDB, organized by simulated "pages" in the URL.
*   **Session History**: Navigation between different saved benchmarks via a history and search system.
*   **Integrated Terminal**: Visual console to view outputs, errors, and system messages during test execution.
*   **Result Visualization**: (Implicit) graphs and operations per second metrics to visually compare results.
*   **Quick Actions**: Shortcuts like `Ctrl + Enter` to run tests quickly.

## How does it work?

The application works entirely in the browser without the need for a backend.

1.  **Global Code**: In the "Globals" panel, write the common setup code. Example:
    ```javascript
    const data = [...Array(1000).keys()];
    ```
2.  **Test Cases**: Create one or more test cases. Each case runs in a separate `Web Worker`.
    *   Case 1: `data.find(x => x == 100)`
    *   Case 2: `data.indexOf(100)`
3.  **Execution**: When pressing "Run Tests", the application:
    *   Takes the global code.
    *   Combines it with each test case.
    *   Executes each case multiple times (or for a fixed time) to calculate Operations per Second (Ops/sec).
    *   Visually displays the results comparing performance.

### Saving System and URLs
The application uses a unique system to "save" states:
*   Each "benchmark" is associated with a page number in the URL (e.g., `?1`, `?2`).
*   When modifying code and saving, the corresponding entry in **IndexedDB** is updated.
*   The "Create New URL" button generates a new clean space (new page) to start a benchmark from scratch.

## Installation and Usage

This project does not require complex Node.js dependencies to run, as it uses native JavaScript (ES Modules).

1.  **Clone the repository** (if applicable).
2.  **Serve the application**: You need a local HTTP server due to the use of ES modules (`import/export`) and Web Workers.
    *   **Option A (Python)**:
        ```bash
        python -m http.server
        ```
    *   **Option B (Node.js/npx)**:
        ```bash
        npx serve .
        ```
    *   **Option C (VS Code)**: Use the "Live Server" extension and open `index.html`.
3.  **Open in browser**: Navigate to `http://localhost:8000/public/` (or the port indicated by your server).

## Project Structure

*   `public/`: Static files
    *   `index.html`: Application entry point.
    *   `worker.js`: Worker responsible for executing test code.
    *   `assets/`: CSS styles.
*   `src/`: JavaScript source code
    *   `index.js`: Main interface logic and coordination.
    *   `app.js`: Database handling (IndexedDB) and navigation.
    *   `components/`: Modular logic for graphs and test cases.
    *   `utils/`: Utilities like `highlight.js` for the editor.
