# deneme
Akciğer Testi No:1
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Akciğer Sağlığı Testi</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: Helvetica, sans-serif;
      background-color: #f4f4f9;
      padding: 20px;
      text-align: center;
    }

    #lungsIcon {
      width: 150px;
      margin: 20px auto;
      animation: pulse 3s ease-in-out infinite;
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.15); }
      100% { transform: scale(1); }
    }

    #lungsIcon img {
      width: 100%;
    }

    #description {
      font-size: 16px;
      margin-bottom: 20px;
      color: #555;
    }

    #counter {
      font-size: 24px;
      margin-top: 10px;
    }

    #lungHealth {
      margin-top: 10px;
      font-weight: bold;
      font-size: 18px;
      color: #333;
    }

    #vo2 {
      font-size: 16px;
      margin-top: 5px;
      color: #333;
    }

    button {
      padding: 10px 20px;
      margin: 10px;
      font-size: 16px;
      border: none;
      border-radius: 5px;
      background-color: #28a745;
      color: white;
      cursor: pointer;
    }

    button:hover {
      background-color: #218838;
    }

    #chartContainer {
      max-width: 600px;
      margin: 30px auto;
    }

    #historyList {
      margin-top: 20px;
      font-size: 16px;
      color: #555;
    }
  </style>
</head>
<body>
  <h2>Akciğer Sağlığı Testi</h2>
  <div id="lungsIcon">
    <img src="https://cdn-icons-png.flaticon.com/512/2927/2927773.png" alt="Ciğer İkonu">
  </div>

  <div id="description">
    Nefesinizi tutun ve <strong>Başlat</strong> düğmesine basın. Nefes verirken <strong>Durdur</strong> düğmesine basın.
  </div>

  <div id="counter">Süre: 0 sn</div>
  <div id="lungHealth"></div>
  <div id="vo2"></div>

  <button onclick="startAnimation()">Başlat</button>
  <button onclick="stopAnimation()">Durdur</button>

  <div id="chartContainer">
    <canvas id="lungChart"></canvas>
  </div>

  <div id="historyList"><strong>Test Geçmişi:</strong><ul id="history"></ul></div>

  <script>
    const counter = document.getElementById('counter');
    const lungHealth = document.getElementById('lungHealth');
    const vo2Element = document.getElementById('vo2');
    const historyList = document.getElementById('history');
    const ctx = document.getElementById('lungChart').getContext('2d');

    let interval;
    let time = 0;
    let testCount = 0;
    const historyData = [];

    const chart = new Chart(ctx, {
      type: 'line',
      data: {
        labels: [],
        datasets: [{
          label: 'Nefes Tutma Süresi (sn)',
          data: [],
          borderColor: '#28a745',
          backgroundColor: 'rgba(40, 167, 69, 0.2)',
          tension: 0.3
        }]
      },
      options: {
        responsive: true,
        scales: {
          y: {
            beginAtZero: true,
            suggestedMax: 60
          }
        }
      }
    });

    function startAnimation() {
      time = 0;
      counter.textContent = `Süre: ${time} sn`;
      lungHealth.textContent = '';
      vo2Element.textContent = '';
      interval = setInterval(() => {
        time++;
        counter.textContent = `Süre: ${time} sn`;
      }, 1000);
    }

    function stopAnimation() {
      clearInterval(interval);
      counter.textContent = `Toplam süre: ${time} sn`;
      displayLungHealth(time);
      estimateVO2(time);
      saveHistory(time);
    }

    function displayLungHealth(duration) {
      let status;
      if (duration < 15) {
        status = "Ciğer sağlığı zayıf (Riskli seviyede)";
      } else if (duration < 30) {
        status = "Ortalama ciğer kapasitesi";
      } else if (duration < 45) {
        status = "İyi ciğer sağlığı";
      } else {
        status = "Harika! Atletik düzeyde ciğer kapasitesi";
      }
      lungHealth.textContent = `Değerlendirme: ${status}`;
    }

    function estimateVO2(duration) {
      const vo2 = Math.round((duration / 60) * 55); // basit tahmini formül
      vo2Element.textContent = `VO₂ max tahmini: ${vo2} ml/kg/dk`;
    }

    function saveHistory(duration) {
      testCount++;
      const label = `Test ${testCount}`;
      historyData.push(duration);
      chart.data.labels.push(label);
      chart.data.datasets[0].data.push(duration);
      chart.update();

      const li = document.createElement('li');
      li.textContent = `${label}: ${duration} sn`;
      historyList.appendChild(li);
    }
  </script>
</body>
</html>
