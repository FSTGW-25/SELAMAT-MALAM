<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Langit Malam Realistik</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background: black;
        }

        canvas {
            display: block;
            position: absolute;
            top: 0;
            left: 0;
        }

        /* Menyembunyikan kontrol audio */
        audio {
            display: none;
        }

        /* Styling tombol play musik */
        #musicButton {
            position: absolute;
            top: 20px;
            left: 20px;
            padding: 10px 20px;
            background-color: rgba(255, 255, 255, 0.6);
            border: none;
            color: black;
            font-size: 16px;
            cursor: pointer;
            border-radius: 5px;
        }

        #musicButton:hover {
            background-color: rgba(255, 255, 255, 0.8);
        }
    </style>
</head>
<body>
    <!-- Menambahkan elemen audio dengan autoplay dan loop -->
    <audio id="backgroundMusic">
        <source src="Coldplay - Fix You.mp3" type="audio/mp3">
        Browser Anda tidak mendukung elemen audio.
    </audio>

    <canvas id="canvas"></canvas>

    <!-- Menambahkan tombol untuk play musik -->
    <button id="musicButton">Play Musik</button>

    <script>
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        const audio = document.getElementById('backgroundMusic');
        const musicButton = document.getElementById('musicButton');

        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        // Posisi bulan dan efek cahaya
        let moonX = canvas.width * 0.7;
        let moonY = canvas.height * 0.3;
        let moonGlow = 150;

        // Bintang dengan variasi ukuran dan efek lebih realistis
        const stars = [];
        for (let i = 0; i < 800; i++) {
            stars.push({
                x: Math.random() * canvas.width,
                y: Math.random() * canvas.height,
                radius: Math.random() * 4 + 0.5,
                opacity: Math.random() * 0.8 + 0.2,
                speed: Math.random() * 0.2 + 0.05
            });
        }

        // Fungsi untuk menggambar bintang segi enam
        function drawHexagon(x, y, radius) {
            const angle = Math.PI / 3; // 60 derajat untuk segi enam
            ctx.beginPath();
            for (let i = 0; i < 6; i++) {
                ctx.lineTo(
                    x + radius * Math.cos(angle * i),
                    y + radius * Math.sin(angle * i)
                );
            }
            ctx.closePath();
            ctx.fill();
        }

        function drawStars() {
            ctx.fillStyle = 'white';
            stars.forEach(star => {
                ctx.globalAlpha = star.opacity;
                drawHexagon(star.x, star.y, star.radius); // Mengganti dengan fungsi hexagon
            });
            ctx.globalAlpha = 1;
        }

        function drawMoon() {
            // Efek cahaya bulan menggunakan gradien
            const gradient = ctx.createRadialGradient(moonX, moonY, 80, moonX, moonY, moonGlow);
            gradient.addColorStop(0, 'rgba(255, 255, 224, 1)'); // Warna cahaya bulan terang
            gradient.addColorStop(1, 'rgba(255, 255, 224, 0)'); // Transparansi cahaya

            // Gambar efek cahaya bulan
            ctx.beginPath();
            ctx.arc(moonX, moonY, moonGlow, 0, Math.PI * 2);
            ctx.fillStyle = gradient;
            ctx.fill();

            // Gambar bulan tanpa gambar eksternal, menggunakan lingkaran dengan tekstur
            ctx.beginPath();
            ctx.arc(moonX, moonY, 80, 0, Math.PI * 2);
            ctx.fillStyle = 'white'; // Warna bulan
            ctx.fill();
        }

        // Tambahkan komet
        const comets = [];
        for (let i = 0; i < 5; i++) {
            comets.push({
                x: Math.random() * canvas.width,
                y: Math.random() * canvas.height / 2,
                length: Math.random() * 50 + 30,
                speed: Math.random() * 5 + 2,
                opacity: Math.random() * 0.5 + 0.5
            });
        }

        function drawComets() {
            comets.forEach(comet => {
                ctx.globalAlpha = comet.opacity;
                ctx.strokeStyle = 'white';
                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.moveTo(comet.x, comet.y);
                ctx.lineTo(comet.x - comet.length, comet.y + comet.length / 3);
                ctx.stroke();
                ctx.globalAlpha = 1;
            });
        }

        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawStars();
            drawMoon();
            drawComets();
            
            // Pergerakan bintang
            stars.forEach(star => {
                star.x -= star.speed;
                if (star.x < 0) {
                    star.x = canvas.width;
                    star.y = Math.random() * canvas.height;
                }
            });
            
            // Pergerakan bulan
            moonX -= 0.02;
            if (moonX < -80) {
                moonX = canvas.width + 80;
            }
            
            // Pergerakan komet
            comets.forEach(comet => {
                comet.x -= comet.speed;
                comet.y += comet.speed / 2;
                if (comet.x < -comet.length || comet.y > canvas.height) {
                    comet.x = canvas.width + Math.random() * 200;
                    comet.y = Math.random() * canvas.height / 2;
                }
            });
            
            requestAnimationFrame(animate);
        }

        // Fungsi untuk kontrol play/pause musik
        musicButton.addEventListener('click', function() {
            if (audio.paused) {
                audio.play();
                musicButton.textContent = "Pause Musik";
            } else {
                audio.pause();
                musicButton.textContent = "Play Musik";
            }
        });

        animate();
    </script>
</body>
</html>
