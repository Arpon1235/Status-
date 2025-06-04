
<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <title>AI স্ট্যাটাস জেনারেটর</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      padding: 30px;
      text-align: center;
      transition: background-color 0.3s, color 0.3s;
    }
    .box {
      background-color: white;
      color: black;
      max-width: 600px;
      margin: auto;
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.1);
    }
    input, button {
      width: 100%;
      padding: 12px;
      margin: 10px 0;
      font-size: 16px;
    }
    button {
      background-color: #4caf50;
      color: white;
      border: none;
      cursor: pointer;
    }
    #output {
      margin-top: 20px;
      font-size: 18px;
      background: #f0f0f0;
      padding: 15px;
      border-radius: 10px;
      text-align: left;
      white-space: pre-wrap;
    }
    .dark-mode {
      background-color: #121212;
      color: white;
    }
    .dark-mode .box {
      background-color: #1e1e1e;
      color: white;
    }
    .dark-mode #output {
      background: #2a2a2a;
      color: #f0f0f0;
    }
  </style>
</head>
<body>

<div class="box">
  <h2>🤖 AI স্ট্যাটাস জেনারেটর</h2>
  <input type="text" id="topic" placeholder="যেমন: ভালোবাসা, জীবন, বন্ধু, একাকীত্ব...">
  <button onclick="generateStatus()">স্ট্যাটাস তৈরি করুন</button>
  <button onclick="copyStatus()">📋 কপি করুন</button>
  <button onclick="toggleDarkMode()">🌙 Dark Mode</button>
  <div id="output">এখানে স্ট্যাটাস আসবে...</div>
</div>

<script>
  async function generateStatus() {
    const topic = document.getElementById("topic").value;
    const outputDiv = document.getElementById("output");
    outputDiv.innerText = "✍️ AI লিখছে... অনুগ্রহ করে অপেক্ষা করুন...";

    const apiKey = "AIzaSyDCf9KxxmHxLYirBIxkNR1fMvJ6ri61TWU";

    const prompt = `Give me a short and emotional Bangla Facebook status on the topic "${topic}". Use beautiful poetic words.`;

    const response = await fetch(
      "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=" + apiKey,
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          contents: [
            {
              parts: [
                { text: prompt }
              ]
            }
          ]
        })
      }
    );

    const data = await response.json();
    try {
      outputDiv.innerText = data.candidates[0].content.parts[0].text;
    } catch (e) {
      outputDiv.innerText = "❌ স্ট্যাটাস তৈরি করা যায়নি। আবার চেষ্টা করুন।";
    }
  }

  function copyStatus() {
    const outputText = document.getElementById("output").innerText;
    navigator.clipboard.writeText(outputText)
      .then(() => alert("✅ কপি করা হয়েছে!"))
      .catch(() => alert("❌ কপি করা যায়নি!"));
  }

  function toggleDarkMode() {
    document.body.classList.toggle("dark-mode");
  }
</script>

</body>
</html>
