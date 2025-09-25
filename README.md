<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GROW A GARDEN SCRIPT 🍓</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      background: #000;
      font-family: 'Poppins', Arial, sans-serif;
      color: #fff;
      text-align: center;
      overflow: hidden;
    }
    canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -2;
    }
    .garden-bg {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: radial-gradient(circle at top, #2ecc71, #1a472a 60%, #000 100%);
      opacity: 0.6;
      z-index: -1;
    }
    .container {
      background: rgba(0, 0, 0, 0.75);
      padding: 30px;
      border-radius: 20px;
      box-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
      max-width: 650px;
      width: 90%;
      z-index: 1;
      animation: float 4s ease-in-out infinite;
      overflow-wrap: break-word;
      word-wrap: break-word;
      word-break: break-word;
    }
    h1 {
      font-size: 3em;
      margin-bottom: 20px;
      text-shadow: 2px 2px 6px #ff4d6d, 0 0 25px #2ecc71;
      background: linear-gradient(90deg, #ff4d6d, #ffcc00, #2ecc71, #3498db);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      animation: rainbow 6s linear infinite;
    }
    .code-box {
      background: #111;
      padding: 15px;
      border-radius: 10px;
      font-family: 'Courier New', monospace;
      font-size: 1.1em;
      color: #0f0;
      margin-bottom: 20px;
      box-shadow: inset 0 0 10px #2ecc71;
      max-width: 100%;
      overflow-x: auto;
      white-space: pre-wrap;
      word-wrap: break-word;
    }
    .copy-btn {
      background: linear-gradient(45deg, #ff4d6d, #ffcc00, #2ecc71);
      color: white;
      border: none;
      padding: 12px 24px;
      font-size: 1.1em;
      font-weight: bold;
      border-radius: 30px;
      cursor: pointer;
      transition: transform 0.2s, box-shadow 0.2s;
    }
    .copy-btn:hover {
      transform: scale(1.08);
      box-shadow: 0 0 20px #ffcc00;
    }
    .copy-btn:active {
      transform: scale(0.95);
      box-shadow: 0 0 10px #2ecc71;
    }
    @keyframes float {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-10px); }
    }
    @keyframes rainbow {
      0% { background-position: 0% 50%; }
      100% { background-position: 100% 50%; }
    }
    .flower {
      position: absolute;
      width: 20px;
      height: 20px;
      border-radius: 50%;
      background: radial-gradient(circle, #ff4d6d, #ff1e4d);
      animation: bloom 5s infinite ease-in-out;
    }
    @keyframes bloom {
      0%, 100% { transform: scale(0.8) rotate(0deg); }
      50% { transform: scale(1.2) rotate(180deg); }
    }
  </style>
</head>
<body>
  <canvas id="stars"></canvas>
  <div class="garden-bg"></div>
  <div class="container">
    <h1>GROW A GARDEN SCRIPT 🍓</h1>
    <div class="code-box" id="code">
      loadstring(game:HttpGet("https://raw.githubusercontent.com/Anirudhapillai/Grow-A-Garden-scripts/refs/heads/main/Protected_8687616688892576.lua.txt"))()
    </div>
    <button class="copy-btn" onclick="copyCode()">🌱 Copy Script</button>
  </div>

  <script>
    // Starfield effect
    const canvas = document.getElementById('stars');
    const ctx = canvas.getContext('2d');
    let stars = [];

    function resizeCanvas() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }
    window.addEventListener('resize', resizeCanvas);
    resizeCanvas();

    function createStar() {
      return {
        x: Math.random() * canvas.width,
        y: Math.random() * canvas.height,
        radius: Math.random() * 1.5,
        opacity: Math.random() * 0.5 + 0.5,
        flickerSpeed: Math.random() * 0.02 + 0.01
      };
    }

    function initStars() {
      stars = [];
      for (let i = 0; i < 250; i++) {
        stars.push(createStar());
      }
    }
    initStars();

    function animate() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      stars.forEach(star => {
        star.opacity += Math.sin(Date.now() * star.flickerSpeed) * 0.02;
        if (star.opacity < 0.2) star.opacity = 0.2;
        if (star.opacity > 1) star.opacity = 1;
        ctx.beginPath();
        ctx.arc(star.x, star.y, star.radius, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(255, 255, 255, ${star.opacity})`;
        ctx.fill();
      });
      requestAnimationFrame(animate);
    }
    animate();

    // Copy code
    function copyCode() {
      const code = document.getElementById('code').innerText;
      if (navigator.clipboard && window.isSecureContext) {
        navigator.clipboard.writeText(code).then(() => {
          alert('🌱 Script copied to clipboard!');
        }).catch(err => {
          console.error('Clipboard API failed: ', err);
          fallbackCopy(code);
        });
      } else {
        fallbackCopy(code);
      }
    }

    function fallbackCopy(text) {
      const textarea = document.createElement('textarea');
      textarea.value = text;
      textarea.style.position = 'fixed';
      textarea.style.opacity = '0';
      document.body.appendChild(textarea);
      textarea.select();
      try {
        document.execCommand('copy');
        alert('🌱 Script copied to clipboard!');
      } catch (err) {
        console.error('Fallback copy failed: ', err);
        alert('Failed to copy script. Please copy manually.');
      }
      document.body.removeChild(textarea);
    }

    // Flower spawning effect
    function spawnFlower() {
      const flower = document.createElement('div');
      flower.className = 'flower';
      flower.style.left = Math.random() * window.innerWidth + 'px';
      flower.style.top = Math.random() * window.innerHeight + 'px';
      document.body.appendChild(flower);
      setTimeout(() => flower.remove(), 7000);
    }
    setInterval(spawnFlower, 1200);
  </script>
</body>
</html>

