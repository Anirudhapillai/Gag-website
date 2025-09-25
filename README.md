<script type="text/javascript">
        var gk_isXlsx = false;
        var gk_xlsxFileLookup = {};
        var gk_fileData = {};
        function filledCell(cell) {
          return cell !== '' && cell != null;
        }
        function loadFileData(filename) {
        if (gk_isXlsx && gk_xlsxFileLookup[filename]) {
            try {
                var workbook = XLSX.read(gk_fileData[filename], { type: 'base64' });
                var firstSheetName = workbook.SheetNames[0];
                var worksheet = workbook.Sheets[firstSheetName];

                // Convert sheet to JSON to filter blank rows
                var jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 1, blankrows: false, defval: '' });
                // Filter out blank rows (rows where all cells are empty, null, or undefined)
                var filteredData = jsonData.filter(row => row.some(filledCell));

                // Heuristic to find the header row by ignoring rows with fewer filled cells than the next row
                var headerRowIndex = filteredData.findIndex((row, index) =>
                  row.filter(filledCell).length >= filteredData[index + 1]?.filter(filledCell).length
                );
                // Fallback
                if (headerRowIndex === -1 || headerRowIndex > 25) {
                  headerRowIndex = 0;
                }

                // Convert filtered JSON back to CSV
                var csv = XLSX.utils.aoa_to_sheet(filteredData.slice(headerRowIndex)); // Create a new sheet from filtered array of arrays
                csv = XLSX.utils.sheet_to_csv(csv, { header: 1 });
                return csv;
            } catch (e) {
                console.error(e);
                return "";
            }
        }
        return gk_fileData[filename] || "";
        }
        </script><!DOCTYPE html>
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
      font-family: Arial, sans-serif;
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
      z-index: -1;
    }
    .container {
      background: rgba(0, 0, 0, 0.7);
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(255, 255, 255, 0.2);
      max-width: 600px;
      width: 90%;
      z-index: 1;
    }
    h1 {
      font-size: 2.5em;
      margin-bottom: 20px;
      text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
    }
    .code-box {
      background: #1e1e1e;
      padding: 15px;
      border-radius: 5px;
      font-family: 'Courier New', monospace;
      font-size: 1em;
      color: #0f0;
      word-wrap: break-word;
      margin-bottom: 20px;
    }
    .copy-btn {
      background: #4CAF50;
      color: white;
      border: none;
      padding: 10px 20px;
      font-size: 1em;
      border-radius: 5px;
      cursor: pointer;
      transition: background 0.3s;
    }
    .copy-btn:hover {
      background: #45a049;
    }
    .copy-btn:active {
      background: #3d8b40;
    }
  </style>
</head>
<body>
  <canvas id="stars"></canvas>
  <div class="container">
    <h1>GROW A GARDEN SCRIPT 🍓</h1>
    <div class="code-box" id="code">
      loadstring(game:HttpGet("https://raw.githubusercontent.com/Anirudhapillai/Grow-A-Garden-scripts/refs/heads/main/Protected_8687616688892576.lua.txt"))()
    </div>
    <button class="copy-btn" onclick="copyCode()">Copy Code</button>
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
      for (let i = 0; i < 200; i++) {
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

    // Copy code function with fallback
    function copyCode() {
      const code = document.getElementById('code').innerText;
      if (navigator.clipboard && window.isSecureContext) {
        // Use modern clipboard API in secure contexts
        navigator.clipboard.writeText(code).then(() => {
          alert('Code copied to clipboard!');
        }).catch(err => {
          console.error('Clipboard API failed: ', err);
          fallbackCopy(code);
        });
      } else {
        // Fallback for non-secure contexts
        fallbackCopy(code);
      }
    }

    function fallbackCopy(text) {
      // Create a temporary textarea element
      const textarea = document.createElement('textarea');
      textarea.value = text;
      textarea.style.position = 'fixed';
      textarea.style.opacity = '0';
      document.body.appendChild(textarea);
      textarea.select();
      try {
        document.execCommand('copy');
        alert('Code copied to clipboard!');
      } catch (err) {
        console.error('Fallback copy failed: ', err);
        alert('Failed to copy code. Please select and copy manually.');
      }
      document.body.removeChild(textarea);
    }
  </script>
</body>
</html>
