# Mini-Rasm-Chizish-Ilovasi-Canvas-API-
HTML Sahifa (index.html)
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Mini Rasm Chizish Ilovasi</title>
    <style>
        body {
            font-family: sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            background: #f4f4f9;
            margin: 0;
            padding: 20px;
        }
        h2 {
            margin-bottom: 10px;
        }
        .toolbar {
            display: flex;
            gap: 15px;
            margin-bottom: 15px;
            align-items: center;
            background: #fff;
            padding: 10px 15px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        canvas {
            border: 2px solid #ccc;
            background: #fff;
            cursor: crosshair;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }
        button {
            padding: 8px 14px;
            cursor: pointer;
            border: none;
            background: #007bff;
            color: white;
            border-radius: 4px;
            font-weight: bold;
        }
        button:hover {
            background: #0056b3;
        }
        .clear-btn {
            background: #dc3545;
        }
        .clear-btn:hover {
            background: #a71d2a;
        }
        .download-btn {
            background: #28a745;
        }
        .download-btn:hover {
            background: #1e7e34;
        }
    </style>
</head>
<body>

    <h2>Mini Rasm Chizish Ilovasi</h2>

    <!-- Boshqaruv vositalari (Toolbar) -->
    <div class="toolbar">
        <label>Rang: <input type="color" id="color-picker" value="#000000"></label>
        <label>Qalinlik: <input type="range" id="size-picker" min="1" max="20" value="3"></label>
        
        <button id="btn-rect">To'rtburchak</button>
        <button id="btn-circle">Doira</button>
        <button id="btn-triangle">Uchburchak</button>
        
        <button id="btn-clear" class="clear-btn">Tozalash</button>
        <button id="btn-download" class="download-btn">Yuklab olish</button>
    </div>

    <!-- 800x600 o'lchamli Canvas -->
    <canvas id="paint-canvas" width="800" height="600"></canvas>

    <!-- JavaScript skripti -->
    <script src="./app.js"></script>
</body>
</html>
2. JavaScript Mantig'i (app.js)
JavaScript
const canvas = document.getElementById('paint-canvas');
const ctx = canvas.getContext('2d');

const colorPicker = document.getElementById('color-picker');
const sizePicker = document.getElementById('size-picker');
const btnRect = document.getElementById('btn-rect');
const btnCircle = document.getElementById('btn-circle');
const btnTriangle = document.getElementById('btn-triangle');
const btnClear = document.getElementById('btn-clear');
const btnDownload = document.getElementById('btn-download');

// Chizish holatini kuzatuvchi o'zgaruvchilar
let isDrawing = false;
let lastX = 0;
let lastY = 0;

// Boshlang'ich canvas sozlamalari
ctx.lineCap = 'round';
ctx.lineJoin = 'round';

// --- 1. ERKIN CHIZISH (Mousedown, Mousemove, Mouseup) ---

canvas.addEventListener('mousedown', (e) => {
    isDrawing = true;
    [lastX, lastY] = [e.offsetX, e.offsetY];
});

canvas.addEventListener('mousemove', (e) => {
    if (!isDrawing) return;

    ctx.beginPath();
    ctx.moveTo(lastX, lastY);
    ctx.lineTo(e.offsetX, e.offsetY);
    
    // Rang va qalinlikni inputlardan olish
    ctx.strokeStyle = colorPicker.value;
    ctx.lineWidth = sizePicker.value;
    
    ctx.stroke();

    [lastX, lastY] = [e.offsetX, e.offsetY];
});

window.addEventListener('mouseup', () => {
    isDrawing = false;
});


// --- 2. GEOMETRIK SHAKLLAR (Tasodifiy joyda) ---

// To'rtburchak chizish
btnRect.addEventListener('click', () => {
    const x = Math.random() * (canvas.width - 150);
    const y = Math.random() * (canvas.height - 150);
    const w = 100;
    const h = 80;

    ctx.fillStyle = colorPicker.value;
    ctx.fillRect(x, y, w, h);
});

// Doira chizish
btnCircle.addEventListener('click', () => {
    const x = Math.random() * (canvas.width - 100) + 50;
    const y = Math.random() * (canvas.height - 100) + 50;
    const r = 40;

    ctx.beginPath();
    ctx.arc(x, y, r, 0, Math.PI * 2);
    ctx.fillStyle = colorPicker.value;
    ctx.fill();
    ctx.strokeStyle = '#000';
    ctx.lineWidth = 2;
    ctx.stroke();
});

// Uchburchak chizish
btnTriangle.addEventListener('click', () => {
    const startX = Math.random() * (canvas.width - 100) + 50;
    const startY = Math.random() * (canvas.height - 100) + 50;

    ctx.beginPath();
    ctx.moveTo(startX, startY);
    ctx.lineTo(startX + 60, startY + 100);
    ctx.lineTo(startX - 60, startY + 100);
    ctx.closePath(); // Yo'lni yopish

    ctx.fillStyle = colorPicker.value;
    ctx.fill();
    ctx.strokeStyle = '#000';
    ctx.lineWidth = 2;
    ctx.stroke();
});


// --- 3. TOZALASH VA YUKLAB OLISH ---

// Canvasni tozalash
btnClear.addEventListener('click', () => {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
});

// Rasm sifatida PNG formatda saqlash
btnDownload.addEventListener('click', () => {
    const dataURL = canvas.toDataURL('image/png');
    
    const link = document.createElement('a');
    link.href = dataURL;
    link.download = 'my-drawing.png';
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
});
