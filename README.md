<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Studio Make Up Glamour - Game Rias Wajah</title>
    <style>
        * {
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background: linear-gradient(145deg, #f8cdda 0%, #f5a9b8 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Segoe UI', 'Quicksand', system-ui, -apple-system, 'Poppins', sans-serif;
            margin: 0;
            padding: 20px;
        }

        /* Container utama */
        .game-container {
            background: #fff0f3;
            border-radius: 60px 60px 50px 50px;
            box-shadow: 0 25px 40px rgba(0,0,0,0.2), inset 0 1px 2px rgba(255,255,255,0.6);
            padding: 20px 25px 30px 25px;
            transition: all 0.2s ease;
        }

        /* Canvas Area */
        .canvas-area {
            display: flex;
            justify-content: center;
            border-radius: 40px;
            background: #ffe8ed;
            padding: 15px;
            box-shadow: inset 0 0 0 3px white, 0 10px 20px rgba(0,0,0,0.1);
        }

        canvas {
            background: #fffaec;
            border-radius: 40px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            cursor: crosshair;
            display: block;
            width: 100%;
            height: auto;
            max-width: 500px;
            aspect-ratio: 1 / 1;
        }

        /* Toolbar Makeup */
        .makeup-tools {
            margin-top: 20px;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 12px;
            background: #ffffffcc;
            backdrop-filter: blur(8px);
            padding: 12px 18px;
            border-radius: 60px;
            box-shadow: 0 8px 18px rgba(0,0,0,0.1);
        }

        .tool-btn {
            background: white;
            border: none;
            font-size: 1.8rem;
            padding: 8px 18px;
            border-radius: 50px;
            font-weight: bold;
            display: inline-flex;
            align-items: center;
            gap: 10px;
            cursor: pointer;
            transition: 0.2s;
            box-shadow: 0 3px 6px rgba(0,0,0,0.1);
            font-family: monospace;
        }

        .tool-btn span {
            font-size: 1rem;
            font-weight: 600;
            color: #b34e6e;
        }

        .tool-btn.active {
            background: #ffb7c5;
            color: white;
            box-shadow: 0 5px 12px #ff8aa3;
            border: 1px solid #ff6f8e;
            transform: scale(0.97);
        }

        .tool-btn.active span {
            color: white;
        }

        /* Ukuran kuas / slider */
        .size-control {
            background: #ffe2e8;
            border-radius: 40px;
            padding: 6px 15px;
            display: flex;
            align-items: center;
            gap: 12px;
            font-weight: bold;
            color: #a13e58;
        }

        .size-control label {
            font-size: 0.9rem;
        }

        input[type="range"] {
            width: 130px;
            cursor: pointer;
        }

        .reset-btn {
            background: #ff8da1;
            color: white;
            border: none;
            padding: 8px 20px;
            border-radius: 40px;
            font-weight: bold;
            font-size: 1rem;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            cursor: pointer;
            transition: 0.2s;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }

        .reset-btn:hover {
            background: #e06e88;
            transform: scale(0.96);
        }

        /* info mode & koordinat kursor */
        .info-panel {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 15px;
            background: #ffffffba;
            border-radius: 40px;
            padding: 6px 18px;
            font-size: 0.8rem;
            color: #8e3e57;
            font-weight: 500;
            backdrop-filter: blur(4px);
        }

        .cursor-pos {
            background: #fbd3dc;
            padding: 4px 12px;
            border-radius: 30px;
            font-family: monospace;
        }

        h2 {
            text-align: center;
            margin: 0 0 8px 0;
            font-size: 1.6rem;
            color: #a13e58;
            text-shadow: 0 2px 3px #ffdae2;
            letter-spacing: 1px;
        }

        .sub {
            text-align: center;
            font-size: 0.75rem;
            margin-top: -5px;
            margin-bottom: 12px;
            color: #c95c78;
        }

        @media (max-width: 550px) {
            .tool-btn {
                padding: 4px 12px;
                font-size: 1.3rem;
            }
            .tool-btn span {
                font-size: 0.7rem;
            }
            .size-control {
                padding: 4px 12px;
            }
            .game-container {
                padding: 15px;
            }
        }
    </style>
</head>
<body>
<div>
    <div class="game-container">
        <h2>💄 GLAM STUDIO 💋</h2>
        <div class="sub">✨ sentuh & gambar makeup dengan kuas ✨</div>

        <div class="canvas-area">
            <canvas id="makeupCanvas" width="500" height="500"></canvas>
        </div>

        <!-- Tools Makeup -->
        <div class="makeup-tools">
            <button id="toolLipstick" class="tool-btn active">💋 <span>Lipstik</span></button>
            <button id="toolBlush" class="tool-btn">🌸 <span>Blush On</span></button>
            <button id="toolEyeshadow" class="tool-btn">✨ <span>Eyeshadow</span></button>
            <button id="toolEyeliner" class="tool-btn">✒️ <span>Eyeliner</span></button>
            <div class="size-control">
                <label>✏️ Ukuran kuas</label>
                <input type="range" id="brushSize" min="5" max="45" value="18" step="1">
                <span id="sizeValue" style="min-width: 35px;">18px</span>
            </div>
            <button id="resetBtn" class="reset-btn">🧽 <span>Hapus Semua Riasan</span></button>
        </div>

        <!-- Panel kursor interaktif dan tombol kursor khusus -->
        <div class="info-panel">
            <div>🎨 Mode aktif: <span id="activeToolName">Lipstik</span></div>
            <div class="cursor-pos">🖱️ Kursor (X,Y): <span id="cursorCoords">0,0</span></div>
            <button id="cursorTrailBtn" style="background: #ffe2ea; border: none; border-radius: 30px; padding: 5px 12px; font-weight: bold; cursor: pointer;">👆 Efek jejak</button>
        </div>
        <div class="sub" style="margin-top: 8px;">💡 Klik & seret di wajah untuk mengaplikasikan makeup | Tombol jejak menunjukkan titik kursor</div>
    </div>
</div>

<script>
    (function() {
        // Ambil elemen canvas
        const canvas = document.getElementById('makeupCanvas');
        const ctx = canvas.getContext('2d');

        // Ukuran canvas fixed 500x500
        canvas.width = 500;
        canvas.height = 500;

        // ------------------- KARAKTER WAJAH DASAR (tanpa makeup) -------------------
        function drawBaseFace() {
            // Bersihkan canvas dengan warna kulit dasar (soft peach)
            ctx.clearRect(0, 0, 500, 500);
            
            // Base kulit
            ctx.fillStyle = '#FDE2CF';
            ctx.fillRect(0, 0, 500, 500);
            
            // Bentuk wajah oval
            ctx.save();
            ctx.shadowBlur = 0;
            ctx.beginPath();
            ctx.ellipse(250, 260, 150, 190, 0, 0, Math.PI * 2);
            ctx.fillStyle = '#F9CFB2';
            ctx.fill();
            ctx.strokeStyle = '#D99E7C';
            ctx.lineWidth = 1.5;
            ctx.stroke();
            
            // Mata kiri dan kanan (sederhana)
            // Mata kiri
            ctx.beginPath();
            ctx.ellipse(195, 220, 18, 22, 0, 0, Math.PI * 2);
            ctx.fillStyle = '#FFFFFF';
            ctx.fill();
            ctx.strokeStyle = '#8B5A3C';
            ctx.lineWidth = 1.2;
            ctx.stroke();
            ctx.fillStyle = '#3E2A1F';
            ctx.beginPath();
            ctx.arc(195, 220, 8, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillStyle = 'white';
            ctx.beginPath();
            ctx.arc(190, 215, 3, 0, Math.PI * 2);
            ctx.fill();
            
            // Mata kanan
            ctx.beginPath();
            ctx.ellipse(305, 220, 18, 22, 0, 0, Math.PI * 2);
            ctx.fillStyle = '#FFFFFF';
            ctx.fill();
            ctx.stroke();
            ctx.fillStyle = '#3E2A1F';
            ctx.beginPath();
            ctx.arc(305, 220, 8, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillStyle = 'white';
            ctx.beginPath();
            ctx.arc(300, 215, 3, 0, Math.PI * 2);
            ctx.fill();
            
            // Alis natural
            ctx.beginPath();
            ctx.moveTo(170, 195);
            ctx.quadraticCurveTo(195, 182, 215, 192);
            ctx.lineWidth = 4;
            ctx.strokeStyle = '#6A4C3A';
            ctx.stroke();
            ctx.beginPath();
            ctx.moveTo(285, 192);
            ctx.quadraticCurveTo(305, 182, 330, 195);
            ctx.stroke();
            
            // Hidung
            ctx.beginPath();
            ctx.moveTo(250, 240);
            ctx.lineTo(245, 280);
            ctx.quadraticCurveTo(250, 288, 255, 280);
            ctx.fillStyle = '#E7B998';
            ctx.fill();
            ctx.strokeStyle = '#C28A6B';
            ctx.lineWidth = 1;
            ctx.stroke();
            
            // Bibir natural (tanpa lipstik)
            ctx.beginPath();
            ctx.ellipse(250, 335, 28, 18, 0, 0, Math.PI * 2);
            ctx.fillStyle = '#E7A78C';
            ctx.fill();
            ctx.strokeStyle = '#B46F53';
            ctx.lineWidth = 1;
            ctx.stroke();
            ctx.beginPath();
            ctx.moveTo(250, 325);
            ctx.lineTo(250, 348);
            ctx.lineWidth = 1.2;
            ctx.stroke();
            
            // Sedikit blush natural (tapi biar lebih simpel, nanti ditimpa mode makeup)
            // Tapi kita gambar blush tipis bawaan? tidak, biar makeup murni dari tools.
            // Tambahkan bulu mata dasar (opsional)
            ctx.fillStyle = '#472E20';
            for(let i=0;i<5;i++) {
                ctx.fillRect(180 + i*3, 228, 1, 5);
                ctx.fillRect(295 + i*3, 228, 1, 5);
            }
            ctx.restore();
        }

        // Simpan layer makeup secara terpisah (agar hapus makeup tidak menghapus wajah)
        let makeupLayer = null;   // ImageData atau kita pakai canvas terpisah lebih mudah
        // Buat offscreen canvas untuk layer makeup
        const offCanvas = document.createElement('canvas');
        offCanvas.width = 500;
        offCanvas.height = 500;
        const offCtx = offCanvas.getContext('2d');
        
        // Inisialisasi layer kosong
        function initMakeupLayer() {
            offCtx.clearRect(0, 0, 500, 500);
            offCtx.fillStyle = 'rgba(0,0,0,0)';
            offCtx.fillRect(0, 0, 500, 500);
            // simpan transparan
            renderFull();
        }
        
        // Render full: wajah + layer makeup (dengan komposit)
        function renderFull() {
            drawBaseFace();  // menggambar ulang wajah di canvas utama
            // di atasnya gambar layer makeup dengan blending normal (alpha blending)
            ctx.drawImage(offCanvas, 0, 0);
        }
        
        // Tools dan warna
        const tools = {
            lipstick: { name: "Lipstik", color: "#E3425E", defaultSize: 18, blendMode: "source-over" },
            blush: { name: "Blush On", color: "#FFA6B0", defaultSize: 28, blendMode: "source-over" },
            eyeshadow: { name: "Eyeshadow", color: "#C46D9E", defaultSize: 20, blendMode: "source-over" },
            eyeliner: { name: "Eyeliner", color: "#2C1E16", defaultSize: 6, blendMode: "source-over" }
        };
        
        let currentTool = "lipstick";   // lipstick default
        let brushSize = 18;
        
        // State drawing
        let isDrawing = false;
        let lastX = 0, lastY = 0;
        
        // Elemen UI
        const toolLipstickBtn = document.getElementById('toolLipstick');
        const toolBlushBtn = document.getElementById('toolBlush');
        const toolEyeshadowBtn = document.getElementById('toolEyeshadow');
        const toolEyelinerBtn = document.getElementById('toolEyeliner');
        const brushSizeSlider = document.getElementById('brushSize');
        const sizeValueSpan = document.getElementById('sizeValue');
        const resetBtn = document.getElementById('resetBtn');
        const activeToolNameSpan = document.getElementById('activeToolName');
        const cursorCoordsSpan = document.getElementById('cursorCoords');
        const cursorTrailBtn = document.getElementById('cursorTrailBtn');
        
        // Update tampilan ukuran
        brushSizeSlider.addEventListener('input', (e) => {
            brushSize = parseInt(e.target.value);
            sizeValueSpan.innerText = brushSize + "px";
        });
        
        // Aktifkan tool
        function setActiveTool(toolId) {
            currentTool = toolId;
            // Update tampilan aktif button
            document.querySelectorAll('.tool-btn').forEach(btn => btn.classList.remove('active'));
            if (toolId === 'lipstick') toolLipstickBtn.classList.add('active');
            if (toolId === 'blush') toolBlushBtn.classList.add('active');
            if (toolId === 'eyeshadow') toolEyeshadowBtn.classList.add('active');
            if (toolId === 'eyeliner') toolEyelinerBtn.classList.add('active');
            let toolName = tools[toolId].name;
            activeToolNameSpan.innerText = toolName;
        }
        
        toolLipstickBtn.addEventListener('click', () => setActiveTool('lipstick'));
        toolBlushBtn.addEventListener('click', () => setActiveTool('blush'));
        toolEyeshadowBtn.addEventListener('click', () => setActiveTool('eyeshadow'));
        toolEyelinerBtn.addEventListener('click', () => setActiveTool('eyeliner'));
        
        // fungsi menggambar di layer makeup
        function applyMakeup(x, y, size, toolType) {
            if (x < 0 || x > 500 || y < 0 || y > 500) return;
            const color = tools[toolType].color;
            offCtx.save();
            offCtx.globalCompositeOperation = "source-over";
            offCtx.beginPath();
            offCtx.arc(x, y, size/2, 0, Math.PI*2);
            offCtx.fillStyle = color;
            offCtx.shadowBlur = 0;
            offCtx.fill();
            // untuk eyeliner sedikit efek solid, untuk blush / eyeshadow pakai opacity halus? supaya lebih natural bisa sedikit transparan. tapi agar terlihat jelas kita pakai opacity 0.85 untuk blush & eyeshadow, lipstik pekat
            if (toolType === 'blush') {
                offCtx.globalAlpha = 0.5;
                offCtx.fillStyle = color;
                offCtx.fill();
                offCtx.globalAlpha = 1.0;
            } else if (toolType === 'eyeshadow') {
                offCtx.globalAlpha = 0.65;
                offCtx.fillStyle = color;
                offCtx.fill();
                offCtx.globalAlpha = 1;
            } else if (toolType === 'lipstick') {
                // Lipstik lebih opaque dengan sedikit efek gradient
                offCtx.fillStyle = color;
                offCtx.fill();
                // tambah sedikit highlight
                offCtx.beginPath();
                offCtx.arc(x-2, y-2, size/5, 0, Math.PI*2);
                offCtx.fillStyle = "#FFC0CB";
                offCtx.globalAlpha = 0.4;
                offCtx.fill();
                offCtx.globalAlpha = 1;
            } else if (toolType === 'eyeliner') {
                offCtx.fillStyle = color;
                offCtx.fill();
            }
            offCtx.restore();
            renderFull();
        }
        
        // Untuk menggambar saat drag dengan interpolasi halus
        function drawOnMakeup(fromX, fromY, toX, toY, size, toolType) {
            const distance = Math.hypot(toX - fromX, toY - fromY);
            if (distance < 0.1) {
                applyMakeup(toX, toY, size, toolType);
                return;
            }
            const steps = Math.max(2, Math.ceil(distance / (size/2)));
            for (let i = 0; i <= steps; i++) {
                const t = i / steps;
                const x = fromX + (toX - fromX) * t;
                const y = fromY + (toY - fromY) * t;
                applyMakeup(x, y, size, toolType);
            }
        }
        
        // Mouse / Touch events
        function getCanvasCoords(e) {
            const rect = canvas.getBoundingClientRect();
            const scaleX = canvas.width / rect.width;   // canvas resolusi 500px, tapi mungkin tampilan responsive
            const scaleY = canvas.height / rect.height;
            let clientX, clientY;
            if (e.touches) {
                if (e.touches.length === 0) return null;
                clientX = e.touches[0].clientX;
                clientY = e.touches[0].clientY;
            } else {
                clientX = e.clientX;
                clientY = e.clientY;
            }
            let canvasX = (clientX - rect.left) * scaleX;
            let canvasY = (clientY - rect.top) * scaleY;
            canvasX = Math.min(Math.max(0, canvasX), 500);
            canvasY = Math.min(Math.max(0, canvasY), 500);
            return { x: canvasX, y: canvasY };
        }
        
        function startDraw(e) {
            e.preventDefault();
            const coord = getCanvasCoords(e);
            if (!coord) return;
            isDrawing = true;
            lastX = coord.x;
            lastY = coord.y;
            applyMakeup(lastX, lastY, brushSize, currentTool);
        }
        
        function drawMove(e) {
            if (!isDrawing) return;
            e.preventDefault();
            const coord = getCanvasCoords(e);
            if (!coord) return;
            const curX = coord.x;
            const curY = coord.y;
            drawOnMakeup(lastX, lastY, curX, curY, brushSize, currentTool);
            lastX = curX;
            lastY = curY;
        }
        
        function endDraw(e) {
            isDrawing = false;
            e.preventDefault();
        }
        
        // Registrasi event mouse & touch
        canvas.addEventListener('mousedown', startDraw);
        window.addEventListener('mousemove', drawMove);
        window.addEventListener('mouseup', endDraw);
        
        canvas.addEventListener('touchstart', startDraw, {passive: false});
        canvas.addEventListener('touchmove', drawMove, {passive: false});
        canvas.addEventListener('touchend', endDraw);
        canvas.addEventListener('touchcancel', endDraw);
        
        // Reset semua makeup (clear layer)
        resetBtn.addEventListener('click', () => {
            offCtx.clearRect(0, 0, 500, 500);
            offCtx.fillStyle = 'rgba(0,0,0,0)';
            offCtx.fillRect(0, 0, 500, 500);
            renderFull();
        });
        
        // Tampilkan koordinat kursor (termasuk untuk mouse dan touch)
        function updateCursorDisplay(e) {
            let coord = null;
            if (e.touches) {
                if (e.touches.length) coord = getCanvasCoords(e);
            } else {
                coord = getCanvasCoords(e);
            }
            if (coord) {
                cursorCoordsSpan.innerText = `${Math.floor(coord.x)}, ${Math.floor(coord.y)}`;
            }
        }
        
        canvas.addEventListener('mousemove', (e) => {
            updateCursorDisplay(e);
        });
        canvas.addEventListener('touchmove', (e) => {
            updateCursorDisplay(e);
            e.preventDefault();
        });
        canvas.addEventListener('mouseenter', (e) => updateCursorDisplay(e));
        
        // Tombol efek jejak kursor: memberikan titik highlight temporer pada makeup (menunjukkan posisi)
        cursorTrailBtn.addEventListener('click', () => {
            // ambil posisi terakhir kursor dari span, parse?
            let coordsText = cursorCoordsSpan.innerText;
            let parts = coordsText.split(',');
            if (parts.length === 2) {
                let x = parseInt(parts[0].trim());
                let y = parseInt(parts[1].trim());
                if (!isNaN(x) && !isNaN(y) && x>=0 && x<=500 && y>=0 && y<=500) {
                    // Buat efek kilau di layer makeup sementara (tapi tidak merusak)
                    offCtx.save();
                    offCtx.globalCompositeOperation = "lighter";
                    offCtx.beginPath();
                    offCtx.arc(x, y, 14, 0, Math.PI*2);
                    offCtx.fillStyle = "#FFF9C4";
                    offCtx.shadowBlur = 0;
                    offCtx.fill();
                    offCtx.restore();
                    renderFull();
                    // hilangkan efek setelah 0.3 detik? supaya tidak ganggu, restore ulang tapi harus backup
                    setTimeout(() => {
                        // kita hapus area sekitar dengan menggambar ulang makeup? agak ribet tetapi kita bisa mengambil snapshot makeup sebelumnya? tapi kita restore dari reinit?
                        // Simpan data sementara: karena efek jelek jika timpa, tapi kita render ulang full dari layer makeup original (offCanvas sebelum ditambah glitter)
                        // Lebih mudah: simpan state offCanvas sebelum glitter lalu restore
                        // Karena kita sudah mengubah offCanvas, lebih baik simpan ImageData sebelum glitter, tapi agar simpel: reload dengan reset lalu gambar ulang tidak praktis.
                        // Alternatif: cukup hapus efek dengan meng-overwrite lingkaran transparan? tapi bisa merusak makeup yang ada di area itu. 
                        // Metode aman: kita bisa buat backup lalu restore setelah timeout. Supaya tidak merusak makeup user:
                        const backupData = offCtx.getImageData(0,0,500,500);
                        // efek kilau sudah ditambahkan sebelumnya di offCanvas. kita restore setelah 300ms ke backup (sebelum glitter)
                        // tunggu sebentar, lalu restore backupData
                        setTimeout(() => {
                            offCtx.putImageData(backupData, 0, 0);
                            renderFull();
                        }, 350);
                    }, 100);
                } else {
                    alert("Gerakkan kursor ke area canvas dulu!");
                }
            } else {
                alert("Arahkan mouse ke area wajah untuk melihat jejak kursor!");
            }
        });
        
        // Inisialisasi awal
        initMakeupLayer();
        
        // Tambahkan optional: jika ingin tombol kursor terpisah bisa ubah kursor canvas, sudah dengan cursor crosshair
        // Juga bisa menambahkan efek pada canvas saat klik tombol jejak.
        
        // Perbaiki saat resize menjaga proporsi canvas asli (tidak perlu karena canvas tetap fixed)
        // Hapus efek shadow berlebih
        console.log("Game Make Up siap! 💄");
    })();
</script>
</body>
</html>
