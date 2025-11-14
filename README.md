<!-- index.html -->
<!DOCTYPE html>
<html lang="fa">
<head>
  <meta charset="UTF-8">
  <title>📍 آب‌وهوا با تشخیص موقعیت</title>
  <style>
    body {
      font-family: sans-serif;
      background: linear-gradient(135deg, #a1c4fd, #c2e9fb);
      height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      margin: 0;
      text-align: center;
    }
    #weather {
      background: rgba(255,255,255,0.9);
      padding: 25px;
      border-radius: 15px;
      width: 300px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.2);
    }
    #weather img {
      width: 80px;
      height: 80px;
    }
    button {
      margin-top: 15px;
      padding: 10px 20px;
      border: none;
      border-radius: 8px;
      background: #333;
      color: white;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <h2>📍 وضعیت آب‌وهوا با موقعیت فعلی</h2>
  <button onclick="getLocation()">دریافت موقعیت من</button>
  <div id="weather">برای شروع، روی دکمه کلیک کنید...</div>

  <script>
    const apiKey = "YOUR_API_KEY"; // کلید API را اینجا قرار بده

    function getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showWeather, showError);
      } else {
        document.getElementById("weather").innerHTML = "مرورگر شما از موقعیت‌یابی پشتیبانی نمی‌کند.";
      }
    }

    async function showWeather(position) {
      const lat = position.coords.latitude;
      const lon = position.coords.longitude;

      try {
        const response = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${apiKey}&units=metric&lang=fa`
        );
        const data = await response.json();

        const icon = `https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;

        document.getElementById("weather").innerHTML = `
          <h3>${data.name}</h3>
          <img src="${icon}" alt="">
          <p>🌡️ دما: ${data.main.temp} °C</p>
          <p>💧 رطوبت: ${data.main.humidity}%</p>
          <p>🌥️ وضعیت: ${data.weather[0].description}</p>
        `;
      } catch (error) {
        document.getElementById("weather").innerHTML = "⚠️ دریافت اطلاعات با خطا مواجه شد";
      }
    }

    function showError(error) {
      document.getElementById("weather").innerHTML = "⚠️ دسترسی به موقعیت مجاز داده نشد یا خطایی رخ داد.";
    }
  </script>

</body>
</html>
