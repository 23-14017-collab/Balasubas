<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Hybrid Solar + Wind Monitoring</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f6f9;
      color: #333;
      margin: 0;
      padding: 0;
    }
    header {
      background: #2b7a78;
      color: #fff;
      padding: 15px;
      text-align: center;
    }
    .container {
      max-width: 900px;
      margin: 20px auto;
      padding: 20px;
      background: #fff;
      border-radius: 12px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    h2 {
      color: #2b7a78;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin: 20px 0;
    }
    table, th, td {
      border: 1px solid #ddd;
      padding: 10px;
      text-align: center;
    }
    th {
      background: #def2f1;
    }
    .buttons {
      text-align: center;
      margin: 20px 0;
    }
    button {
      background: #2b7a78;
      color: #fff;
      padding: 12px 20px;
      border: none;
      border-radius: 8px;
      margin: 5px;
      cursor: pointer;
      font-size: 16px;
    }
    button:hover {
      background: #205b57;
    }
    footer {
      text-align: center;
      padding: 10px;
      background: #def2f1;
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <header>
    <h1>🌞 Hybrid Solar + Wind Dashboard 🌬️</h1>
    <p>Monitor & Control Anywhere via WiFi</p>
  </header>

  <div class="container">
    <h2>System Telemetry</h2>
    <table>
      <tr><th>Parameter</th><th>Value</th></tr>
      <tr><td>Voltage (V)</td><td id="voltage">--</td></tr>
      <tr><td>Current (A)</td><td id="current">--</td></tr>
      <tr><td>Temperature (°C)</td><td id="temp">--</td></tr>
      <tr><td>RPM (Wind)</td><td id="rpm">--</td></tr>
      <tr><td>Inverter</td><td id="inverter">--</td></tr>
      <tr><td>Main Contactor</td><td id="main">--</td></tr>
    </table>

    <div class="buttons">
      <button onclick="sendCmd('REMOTE_ON')">🔵 Remote ON</button>
      <button onclick="sendCmd('REMOTE_OFF')">🔴 Remote OFF</button>
    </div>
  </div>

  <footer>
    Hybrid Solar + Wind Project | Remote Monitoring & Control
  </footer>

  <script>
    const BASE_URL = window.location.origin;

    function updateTelemetry() {
      fetch(BASE_URL + "/telemetry")
        .then(res => res.json())
        .then(data => {
          document.getElementById("voltage").textContent = data.voltage.toFixed(2);
          document.getElementById("current").textContent = data.current.toFixed(2);
          document.getElementById("temp").textContent = data.temp.toFixed(1);
          document.getElementById("rpm").textContent = data.rpm;
          document.getElementById("inverter").textContent = data.inverter ? "ON" : "OFF";
          document.getElementById("main").textContent = data.main ? "Closed" : "Open";
        })
        .catch(err => console.error("Telemetry fetch failed:", err));
    }

    function sendCmd(cmd) {
      fetch(BASE_URL + "/cmd?c=" + cmd)
        .then(res => res.text())
        .then(msg => alert(msg));
    }

    setInterval(updateTelemetry, 2000);
    updateTelemetry();
  </script>
</body>
</html>
