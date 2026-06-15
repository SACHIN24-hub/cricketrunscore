<!DOCTYPE html>
<html>
<head>
  <title>Cricket Scoreboard</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
      color: #fff;
      text-align: center;
      margin: 0;
      padding: 20px;
    }
    .scoreboard {
      background: rgba(255,255,255,0.1);
      border-radius: 12px;
      padding: 20px;
      max-width: 750px;
      margin: auto;
      box-shadow: 0 0 15px rgba(0,0,0,0.7);
    }
    h2 {
      margin-bottom: 10px;
      color: #ffd700;
    }
    .section {
      margin: 15px 0;
    }
    button {
      margin: 5px;
      padding: 10px 15px;
      border: none;
      border-radius: 6px;
      background: #ffd700;
      color: #000;
      font-weight: bold;
      cursor: pointer;
    }
    button:hover {
      background: #ff4500;
      color: #fff;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 15px;
    }
    th, td {
      border: 1px solid #fff;
      padding: 6px;
      text-align: center;
    }
    th {
      background: #ffd700;
      color: #000;
    }
  </style>
</head>
<body>
  <div class="scoreboard">
    <h2>🏏 Cricket Scoreboard</h2>
    <div class="section">
      <strong>Date:</strong> <span id="matchDate"></span>
    </div>
    <div class="section">
      <strong>Score:</strong> <span id="runs">0</span>/<span id="wickets">0</span>
    </div>
    <div class="section">
      <strong>Overs:</strong> <span id="overs">0</span>.<span id="balls">0</span>
    </div>
    <div class="section">
      <button onclick="addRun(1)">+1 Run</button>
      <button onclick="addRun(2)">+2</button>
      <button onclick="addRun(3)">+3</button>
      <button onclick="addRun(4)">+4</button>
      <button onclick="addRun(5)">+5</button>
      <button onclick="addRun(6)">+6</button>
      <button onclick="addWicket()">Out</button>
    </div>
    <div class="section">
      <button onclick="extra('Wide')">Wide</button>
      <button onclick="extra('No Ball')">No Ball</button>
      <button onclick="extra('Dead Ball')">Dead Ball</button>
      <button onclick="reverseOut()">Reverse Out</button>
      <button onclick="setStatus('Not Out')">Not Out</button>
    </div>
    <div class="section">
      <strong>Extras:</strong> <span id="extras"></span>
    </div>
    <div class="section">
      <strong>Status:</strong> <span id="status">Match in Progress</span>
    </div>
    <div class="section">
      <h3>Ball-by-Ball Log</h3>
      <table>
        <thead>
          <tr>
            <th>Over.Ball</th>
            <th>Result</th>
          </tr>
        </thead>
        <tbody id="ballLog"></tbody>
      </table>
    </div>
    <div class="section">
      <h3>Batsman Runs</h3>
      <table>
        <thead>
          <tr>
            <th>Batsman</th>
            <th>Runs</th>
          </tr>
        </thead>
        <tbody id="batsmanTable">
          <tr><td>Batsman 1</td><td id="batsman1Runs">0</td></tr>
          <tr><td>Batsman 2</td><td id="batsman2Runs">0</td></tr>
        </tbody>
      </table>
      <br>
      <button onclick="addBatsmanRun(1,1)">+1 (Batsman 1)</button>
      <button onclick="addBatsmanRun(1,2)">+2 (Batsman 1)</button>
      <button onclick="addBatsmanRun(1,3)">+3 (Batsman 1)</button>
      <button onclick="addBatsmanRun(1,4)">+4 (Batsman 1)</button>
      <button onclick="addBatsmanRun(1,6)">+6 (Batsman 1)</button>
      <br>
      <button onclick="addBatsmanRun(2,1)">+1 (Batsman 2)</button>
      <button onclick="addBatsmanRun(2,2)">+2 (Batsman 2)</button>
      <button onclick="addBatsmanRun(2,3)">+3 (Batsman 2)</button>
      <button onclick="addBatsmanRun(2,4)">+4 (Batsman 2)</button>
      <button onclick="addBatsmanRun(2,6)">+6 (Batsman 2)</button>
    </div>
  </div>

  <script>
    let runs = 0;
    let wickets = 0;
    let balls = 0;
    let overs = 0;
    let extrasList = [];
    let batsmanRuns = {1:0, 2:0};

    document.getElementById("matchDate").innerText = new Date().toLocaleDateString();

    function logBall(result) {
      const log = document.getElementById("ballLog");
      const row = document.createElement("tr");
      row.innerHTML = `<td>${overs}.${balls}</td><td>${result}</td>`;
      log.appendChild(row);
    }

    function addRun(num) {
      runs += num;
      document.getElementById("runs").innerText = runs;
      addBall();
      logBall(num + " run(s)");
    }

    function addBatsmanRun(batsman, num) {
      batsmanRuns[batsman] += num;
      runs += num;
      document.getElementById("runs").innerText = runs;
      document.getElementById("batsman" + batsman + "Runs").innerText = batsmanRuns[batsman];
      addBall();
      logBall(num + " run(s) by Batsman " + batsman);
    }

    function addWicket() {
      wickets++;
      document.getElementById("wickets").innerText = wickets;
      addBall();
      logBall("Wicket");
      setStatus("Out");
    }

    function reverseOut() {
      if (wickets > 0) {
        wickets--;
        document.getElementById("wickets").innerText = wickets;
        setStatus("Decision Reversed");
        logBall("Out Reversed");
      }
    }

    function addBall() {
      balls++;
      if (balls === 6) {
        overs++;
        balls = 0;
      }
      document.getElementById("overs").innerText = overs;
      document.getElementById("balls").innerText = balls;
    }

    function extra(type) {
      extrasList.push(type);
      document.getElementById("extras").innerText = extrasList.join(", ");
      if (type === "Wide" || type === "No Ball") {
        runs++;
        document.getElementById("runs").innerText = runs;
        logBall(type + " (+1 run)");
      } else {
        logBall(type);
      }
    }

    function setStatus(state) {
      document.getElementById("status").innerText = state;
    }
  </script>
</body>
</html>
