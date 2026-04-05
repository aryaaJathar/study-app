<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chill Study Zone</title>
    <style>
        :root {
            --glass: rgba(255, 255, 255, 0.2);
            --text: #2d3436;
            --accent: #a29bfe;
            --bg-gradient: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%);
        }

        body {
            font-family: 'Quicksand', sans-serif;
            background: var(--bg-gradient);
            background-attachment: fixed;
            color: var(--text);
            margin: 0;
            display: flex;
            justify-content: center;
            padding: 40px 20px;
        }

        .glass-panel {
            background: var(--glass);
            backdrop-filter: blur(15px);
            border: 1px solid rgba(255,255,255,0.3);
            border-radius: 24px;
            padding: 30px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 900px;
        }

        header { text-align: center; margin-bottom: 40px; }
        h1 { font-weight: 300; letter-spacing: 2px; }

        .grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .card {
            background: rgba(255,255,255,0.4);
            padding: 20px;
            border-radius: 18px;
            margin-bottom: 20px;
        }

        /* --- MCQ GAME AREA --- */
        #game-canvas {
            background: #2d3436;
            width: 100%;
            height: 200px;
            border-radius: 12px;
            position: relative;
            overflow: hidden;
            display: none;
        }

        .player {
            width: 30px;
            height: 30px;
            background: var(--accent);
            position: absolute;
            bottom: 20px;
            left: 50px;
            border-radius: 5px;
            transition: 0.3s;
        }

        /* --- CALENDAR --- */
        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 5px;
            text-align: center;
            font-size: 0.8rem;
        }

        .day {
            padding: 10px;
            background: rgba(255,255,255,0.2);
            border-radius: 5px;
        }

        input, button {
            padding: 12px;
            border-radius: 10px;
            border: none;
            margin: 5px 0;
        }

        button {
            background: var(--accent);
            color: white;
            cursor: pointer;
            font-weight: bold;
        }

        .hidden { display: none; }
    </style>
</head>
<body>

<div class="glass-panel">
    <header>
        <h1>Cloud Study ☁️</h1>
        <p>Take a breath. You've got this.</p>
    </header>

    <div class="grid">
        <section>
            <div class="card">
                <h3>🧠 AI Quiz Lab</h3>
                <p style="font-size: 0.9rem;">Upload notes to generate a game.</p>
                <input type="file" id="noteUpload" accept="image/*">
                <button onclick="simulateAIParsing()">Generate Game</button>
                
                <div id="game-container" class="hidden">
                    <p id="questionText">Question will appear here...</p>
                    <div id="game-canvas">
                        <div class="player" id="player"></div>
                    </div>
                    <div style="display:flex; gap:10px; margin-top:10px;">
                        <button onclick="movePlayer('left')">Option A</button>
                        <button onclick="movePlayer('right')">Option B</button>
                    </div>
                </div>
            </div>
        </section>

        <section>
            <div class="card">
                <h3>📅 Exam Countdown</h3>
                <input type="text" id="examName" placeholder="Exam Name (e.g. Math)">
                <input type="date" id="examDate">
                <button onclick="addExam()">Add Exam</button>
                
                <ul id="examList" style="list-style:none; padding:0;"></ul>
            </div>

            <div class="card">
                <h3>🗓️ Smart Schedule</h3>
                <div class="calendar-grid" id="calendar">
                    </div>
            </div>
        </section>
    </div>
</div>

<script>
    // --- 1. GAME LOGIC (Simulation) ---
    function simulateAIParsing() {
        alert("AI is 'reading' your image... (Simulated)");
        document.getElementById('game-container').classList.remove('hidden');
        document.getElementById('game-canvas').style.display = 'block';
        document.getElementById('questionText').innerText = "Solve for X: 2x + 5 = 15. Move to the correct answer!";
    }

    function movePlayer(dir) {
        const player = document.getElementById('player');
        if(dir === 'left') {
            player.style.left = '50px';
            alert("Correct! You reached Option A (X=5)");
        } else {
            player.style.left = '200px';
            alert("Wrong! Option B was 10.");
        }
    }

    // --- 2. EXAM & CALENDAR LOGIC ---
    let exams = [];

    function addExam() {
        const name = document.getElementById('examName').value;
        const date = new Date(document.getElementById('examDate').value);
        const today = new Date();
        
        const diffTime = Math.abs(date - today);
        const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)); 

        if(name && date) {
            const entry = `<li><strong>${name}</strong>: ${diffDays} days left!</li>`;
            document.getElementById('examList').innerHTML += entry;
            generateSchedule(diffDays, name);
        }
    }

    function generateSchedule(days, subject) {
        const calendar = document.getElementById('calendar');
        calendar.innerHTML = '';
        for(let i=1; i<=30; i++) {
            const dayDiv = document.createElement('div');
            dayDiv.className = 'day';
            dayDiv.innerText = i;
            if(i < days && i % 2 === 0) {
                dayDiv.style.background = '#fab1a0'; // Study sessions
                dayDiv.title = `Study ${subject}`;
            }
            calendar.appendChild(dayDiv);
        }
    }

    // Initialize blank calendar
    generateSchedule(0, '');
</script>

</body>
</html>
