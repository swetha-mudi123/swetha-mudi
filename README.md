# swetha-mudi
HTML CODE:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stopwatch</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="stopwatch-container">
        <h1>Stopwatch</h1>
        <div id="display">00:00:00.000</div>
        <div class="buttons">
            <button id="start-stop">Start</button>
            <button id="reset">Reset</button>
            <button id="lap">Lap</button>
        </div>
        <div id="laps">
            <h2>Laps</h2>
            <ul id="lap-times"></ul>
        </div>
    </div>
    
    <script src="script.js"></script>
</body>
</html>
CSS CODE:
body {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
    margin: 0;
}

.stopwatch-container {
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    text-align: center;
}

#display {
    font-size: 2.5em;
    margin-bottom: 20px;
}

.buttons button {
    font-size: 1.2em;
    padding: 10px 20px;
    margin: 5px;
    border: none;
    border-radius: 5px;
    background-color: #4CAF50;
    color: white;
    cursor: pointer;
}

.buttons button:hover {
    background-color: #45a049;
}

#lap {
    background-color: #2196F3;
}

#lap:hover {
    background-color: #0b7dda;
}

#reset {
    background-color: #f44336;
}

#reset:hover {
    background-color: #da190b;
}

#laps ul {
    list-style-type: none;
    padding: 0;
}

#laps ul li {
    background-color: #e0e0e0;
    padding: 5px;
    margin: 5px;
    border-radius: 5px;
}
JAVA SCRIPT CODE:
let startTime = 0;
let updatedTime = 0;
let difference = 0;
let isRunning = false;
let interval;
let laps = [];

const display = document.getElementById('display');
const startStopButton = document.getElementById('start-stop');
const resetButton = document.getElementById('reset');
const lapButton = document.getElementById('lap');
const lapTimes = document.getElementById('lap-times');

// Function to start or stop the stopwatch
function startStop() {
    if (isRunning) {
        clearInterval(interval);
        startStopButton.textContent = 'Start';
    } else {
        startTime = Date.now() - difference;
        interval = setInterval(updateDisplay, 10);
        startStopButton.textContent = 'Stop';
    }
    isRunning = !isRunning;
}

// Function to reset the stopwatch
function reset() {
    clearInterval(interval);
    isRunning = false;
    difference = 0;
    display.textContent = '00:00:00.000';
    startStopButton.textContent = 'Start';
    lapTimes.innerHTML = '';
    laps = [];
}

// Function to record a lap
function lap() {
    if (isRunning) {
        const lapTime = difference;
        laps.push(lapTime);
        renderLapTimes();
    }
}

// Function to update the stopwatch display
function updateDisplay() {
    updatedTime = Date.now();
    difference = updatedTime - startTime;

    const milliseconds = difference % 1000;
    const seconds = Math.floor(difference / 1000) % 60;
    const minutes = Math.floor(difference / (1000 * 60)) % 60;
    const hours = Math.floor(difference / (1000 * 60 * 60));

    display.textContent = 
        `${pad(hours)}:${pad(minutes)}:${pad(seconds)}.${pad(milliseconds, 3)}`;
}

// Helper function to pad numbers with leading zeros
function pad(number, digits = 2) {
    return number.toString().padStart(digits, '0');
}

// Function to render lap times
function renderLapTimes() {
    lapTimes.innerHTML = laps.map((lap, index) => {
        const milliseconds = lap % 1000;
        const seconds = Math.floor(lap / 1000) % 60;
        const minutes = Math.floor(lap / (1000 * 60)) % 60;
        const hours = Math.floor(lap / (1000 * 60 * 60));

        return `<li>Lap ${index + 1}: ${pad(hours)}:${pad(minutes)}:${pad(seconds)}.${pad(milliseconds, 3)}</li>`;
    }).join('');
}

// Event listeners
startStopButton.addEventListener('click', startStop);
resetButton.addEventListener('click', reset);
lapButton.addEventListener('click', lap);
