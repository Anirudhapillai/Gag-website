<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GROW A GARDEN SCRIPT 🍓</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'Poppins', Arial, sans-serif;
      background: radial-gradient(circle at top, #2ecc71, #1a472a 60%, #000 100%);
      color: #fff;
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }
    canvas, .particles, .butterfly, .leaf {
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      pointer-events: none;
      z-index: -3;
    }
    .garden-bg {
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: radial-gradient(circle at center, rgba(46,204,113,0.4), transparent 70%);
      z-index: -2;
    }
    .container {
      background: rgba(0, 0, 0, 0.7);
      padding: 40px;
      border-radius: 25px;
      box-shadow: 0 0 40px rgba(46, 204, 113, 0.6);
      max-width: 700px;
      width: 90%;
      text-align: center;
      animation: float 4s ease-in-out infinite;
      backdrop-filter: blur(10px);
      position: relative;
      z-index: 2;
    }
    h1 {
      font-size: 3.5em;
      margin-bottom: 25px;
      background: linear-gradient(90deg, #ff4d6d, #ffcc00, #2ecc71, #3498db, #9b59b6);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-size: 400% 400%;
      animation: rainbow 8s linear infinite, glow 3s ease-in-out infinite;
      text-shadow: 0 0 15px rgba(255,255,255,0.4);
    }
    .code-box {
      background: #111;
      padding: 20px;
      border-radius: 15px;
      font-family: 'Courier New', monospace;
      font-size: 1.2em;
      color: #0f0;
      margin-bottom: 25px;
      box-shadow: inset 0 0 15px #2ecc71;
      white-space: pre-wrap;
      word-wrap: break-word;
    }
    .copy-btn {
      background: linear-gradient(45deg, #ff4d6d, #ffcc00, #2ecc71);
      color: #fff;
      padding: 14px 30px;
      border: none;
      border-radius: 40px;
      font-size: 1.2em;
      font-weight: bold;
      cursor: pointer;
      transition: transform 0.2s, box-shadow 0.2s;
      position: relative;
      overflow: hidden;
    }
    .copy-btn::after {
      content: "";
      position: absolute;
      width: 300%;
      height: 300%;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%) scale(0);
      background: rgba(255,255,255,0.3);
      border-radius: 50%;
      transition: transform 0.5s;
      z-index: 0;
    }
    .copy-btn:hover { transform: scale(1.08); box-shadow: 0 0 25px #ffcc00; }
    .copy-btn:active::after { transform: translate(-50%, -50%) scale(1); }
    .toast {
      position: fixed;
      bottom: 30px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(0,0,0,0.85);
      padding: 15px 25px;
      border-radius: 12px;
      font-size: 1.1em;
      color: #2ecc71;
      opacity: 0;
      transition: opacity 0.5s, bottom 0.5s;
      z-index: 5;
    }
    .toast.show { opacity: 1; bottom: 60px; }
    @keyframes float { 0%,100%{transform:translateY(0);}50%{transform:translateY(-10px);} }
    @keyframes rainbow { 0%{background-position:0% 50%;}100%{background-position:100% 50%;} }
    @keyframes glow { 0%,100%{text-shadow:0 0 15px rgba(255,255,255,0.4);}50%{text-shadow:0 0 30px rgba(255,255,255,0.9);} }
    .flower {
      position: absolute;
      width: 22px; height: 22px;
      border-radius: 50%;
      background: radial-gradient(circle, #ff4d6d, #ff1e4d);
      animation: bloom 6s infinite ease-in-out;
      pointer-events: none;
    }
    @keyframes bloom { 0%,100%{transform:scale(0.8) rotate(0);}50%{transform:scale(1.4) rotate(180deg);} }
    .leaf, .butterfly {
      position: absolute;
      width: 30px; height: 30px;
      background-size: cover;
      animation: drift 20s linear infinite;
    }
    .leaf { background-image: url('https://i.ibb.co/rpJfn4F/leaf.png'); opacity: 0.7; }
    .butterfly { background-image: url('https://i.ibb.co/MBsc6MQ/butterfly.gif'); width: 40px; height: 40px; }
    @keyframes drift { from{transform:translateY(-10%) rotate(0);}to{transform:translateY(110%) rotate(360deg);} }
  </style>
</head>
<body>
  <canvas id="stars"></canvas>
  <div class="garden-bg"></div>
  <div class="container">
    <h1>GROW A GARDEN SCRIPT 🍓</h1>
    <div class="code-box" id="code">
      loadstring(game:HttpGet("https://raw.githubusercontent.com/Anirudhapillai/Grow-a-garden-script/refs/heads/main/Growgarden2script.txt"))() 
    </div>
    <button class="copy-btn" onclick="copyCode()">🌱 Copy Script</button>
  </div>
  <div class="toast" id="toast">🌱 Script copied!</div>

  <script>
    // Stars background
    const canvas = document.getElementById('stars');
    const ctx = canvas.getContext('2d');
    let stars = [];
    function resizeCanvas(){canvas.width=innerWidth;canvas.height=innerHeight;}
    window.addEventListener('resize',resizeCanvas);resizeCanvas();
    function createStar(){return{x:Math.random()*canvas.width,y:Math.random()*canvas.height,radius:Math.random()*1.5,opacity:Math.random()*0.5+0.5,speed:Math.random()*0.5+0.2};}
    function initStars(){stars=[];for(let i=0;i<250;i++){stars.push(createStar());}}
    function animate(){ctx.clearRect(0,0,canvas.width,canvas.height);stars.forEach(star=>{ctx.beginPath();ctx.arc(star.x,star.y,star.radius,0,Math.PI*2);ctx.fillStyle=`rgba(255,255,255,${star.opacity})`;ctx.fill();star.y+=star.speed;if(star.y>canvas.height)star.y=0;});requestAnimationFrame(animate);}initStars();animate();

    // Copy code
    function copyCode(){
      const code=document.getElementById('code').innerText;
      navigator.clipboard.writeText(code).then(()=>{
        const toast=document.getElementById('toast');
        toast.classList.add('show');
        setTimeout(()=>toast.classList.remove('show'),2000);
      }).catch(()=>{
        alert('Copy failed. Please copy manually.');
      });
    }

    // Spawn flowers
    function spawnFlower(){const f=document.createElement('div');f.className='flower';f.style.left=Math.random()*innerWidth+'px';f.style.top=Math.random()*innerHeight+'px';document.body.appendChild(f);setTimeout(()=>f.remove(),8000);}setInterval(spawnFlower,1200);

    // Falling leaves and butterflies
    function spawnLeaf(){const l=document.createElement('div');l.className='leaf';l.style.left=Math.random()*innerWidth+'px';l.style.animationDuration=(15+Math.random()*10)+'s';document.body.appendChild(l);setTimeout(()=>l.remove(),25000);}setInterval(spawnLeaf,3000);
    function spawnButterfly(){const b=document.createElement('div');b.className='butterfly';b.style.left=Math.random()*innerWidth+'px';b.style.animationDuration=(10+Math.random()*10)+'s';document.body.appendChild(b);setTimeout(()=>b.remove(),20000);}setInterval(spawnButterfly,8000);
  </script>
</body>
</html>
