<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>秋季 GIF 產生器</title>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      padding: 2em;
      background: #f9f2ea;
    }
    input, button {
      padding: 0.5em;
      margin: 1em;
    }
    #result-img {
      max-width: 80%;
      border: 2px solid #ccc;
      border-radius: 8px;
      display: none;
    }
  </style>
</head>
<body>
  <h1>🍁 秋季 GIF 產生器</h1>
  <p>輸入一句話，AI 將為你產生一張秋季風格的圖片：</p>

  <input type="text" id="prompt-input" placeholder="例如：一隻橘貓在落葉中打滾">
  <button id="generate-btn">✨ 生成圖片</button>

  <h3 id="loading-text" style="display: none;">正在產生圖片，請稍候...</h3>
  <img id="result-img" />

  <script>
    const API_URL = 'https://你的-subdomain.workers.dev'; // ← 替換成你的 Worker 網址
    const btn = document.getElementById('generate-btn');
    const input = document.getElementById('prompt-input');
    const resultImg = document.getElementById('result-img');
    const loadingText = document.getElementById('loading-text');

    btn.onclick = async () => {
      const prompt = input.value.trim();
      if (!prompt) return alert("請先輸入提示詞");

      resultImg.style.display = 'none';
      loadingText.style.display = 'block';

      try {
        const res = await fetch(API_URL, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ prompt })
        });

        const data = await res.json();
        if (data.imageUrl) {
          resultImg.src = data.imageUrl;
          resultImg.style.display = 'block';
        } else {
          alert("生成失敗：" + (data.error || "未知錯誤"));
        }
      } catch (err) {
        alert("發生錯誤：" + err.
