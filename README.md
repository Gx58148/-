<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#020205">
    <title>Crystal Clock 8K</title>
    
    <!-- تهيئة التطبيق (PWA) -->
    <link rel="manifest" href="data:application/json;base64,ewogICJuYW1lIjogIkNyeXN0YWwgQ2xvY2sgOEsiLAogICJzaG9ydF9uYW1lIjogIkNyeXN0YWwiLAogICJzdGFydF91cmwiOiAiLiIsCiAgImRpc3BsYXkiOiAic3RhbmRhbG9uZSIsCiAgImJhY2tncm91bmRfY29sb3IiOiAiIzAyMDIwNSIsCiAgInRoZW1lX2NvbG9yIjogIiMwMjAyMDUiCn0=">

    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@900&display=swap');

        :root {
            --glass-bg: rgba(255, 255, 255, 0.03);
            --glass-border: rgba(255, 255, 255, 0.2);
        }

        body {
            background-color: #020205;
            background-image: 
                radial-gradient(circle at 20% 20%, rgba(79, 70, 229, 0.35) 0%, transparent 50%),
                radial-gradient(circle at 80% 80%, rgba(190, 24, 93, 0.35) 0%, transparent 50%),
                url('https://images.unsplash.com/photo-1639606033411-93558d60e684?q=80&w=2560&auto=format&fit=crop');
            background-size: cover;
            background-position: center;
            height: 100vh;
            margin: 0;
            font-family: 'Inter', sans-serif;
            display: flex;
            justify-content: center;
            overflow: hidden;
            perspective: 2000px;
            touch-action: none; /* منع التمرير العشوائي في التطبيق */
        }

        .main-wrapper {
            width: 100%;
            max-width: 414px;
            padding: env(safe-area-inset-top) 20px env(safe-area-inset-bottom) 20px;
            display: flex;
            flex-direction: column;
            gap: 22px;
            justify-content: space-around;
            height: 100%;
            z-index: 10;
        }

        .glass-clock-container {
            position: relative;
            padding: 45px 15px;
            border-radius: 56px;
            background: linear-gradient(135deg, rgba(255,255,255,0.05) 0%, rgba(255,255,255,0.01) 100%);
            backdrop-filter: blur(80px) saturate(300%) contrast(150%);
            -webkit-backdrop-filter: blur(80px) saturate(300%) contrast(150%);
            border: 1px solid var(--glass-border);
            box-shadow: 0 60px 120px rgba(0,0,0,0.8), inset 0 0 50px rgba(255,255,255,0.1);
            text-align: center;
            overflow: hidden;
            transform-style: preserve-3d;
            transition: transform 0.2s cubic-bezier(0.1, 0.8, 0.2, 1);
        }

        .light-refractor {
            position: absolute;
            width: 400px;
            height: 400px;
            background: radial-gradient(circle, rgba(255,255,255,0.2) 0%, transparent 75%);
            border-radius: 50%;
            pointer-events: none;
            mix-blend-mode: color-dodge;
            z-index: 1;
            opacity: 0;
            transition: opacity 0.5s;
        }

        .iphone-glass-clock {
            font-size: 7.5rem;
            font-weight: 900;
            line-height: 0.75;
            letter-spacing: -8px;
            color: transparent;
            background: linear-gradient(180deg, #fff 0%, rgba(255,255,255,0.3) 48%, rgba(255,255,255,0.6) 52%, #fff 100%);
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-stroke: 1.5px rgba(255, 255, 255, 0.5);
            filter: drop-shadow(0 30px 50px rgba(0,0,0,0.8));
            z-index: 5;
            position: relative;
        }

        .date-label {
            font-size: 1.3rem;
            font-weight: 800;
            color: rgba(255,255,255,0.9);
            margin-bottom: 15px;
            display: block;
            text-shadow: 0 10px 25px rgba(0,0,0,0.8);
        }

        .widget-card {
            background: var(--glass-bg);
            backdrop-filter: blur(40px) saturate(200%);
            -webkit-backdrop-filter: blur(40px) saturate(200%);
            border: 1px solid rgba(255,255,255,0.15);
            border-radius: 42px;
            padding: 24px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.4);
        }

        .dock {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(55px) saturate(250%);
            border-radius: 44px;
            padding: 12px;
            display: flex;
            justify-content: space-around;
            border: 1px solid rgba(255,255,255,0.2);
            box-shadow: 0 30px 60px rgba(0,0,0,0.6);
        }

        .icon-box {
            width: 62px; height: 62px;
            border-radius: 20px;
            display: flex; align-items: center; justify-content: center;
            font-size: 30px; color: white;
            transition: 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        .icon-box:active { transform: scale(0.8); }

        .sun-rays {
            position: absolute;
            top: 50%; left: 50%;
            width: 150%; height: 150%;
            background: radial-gradient(circle, rgba(250,204,21,0.15) 0%, transparent 70%);
            transform: translate(-50%, -50%);
            animation: pulse 4s infinite ease-in-out;
        }

        @keyframes pulse {
            0%, 100% { opacity: 0.4; transform: translate(-50%, -50%) scale(1); }
            50% { opacity: 0.8; transform: translate(-50%, -50%) scale(1.1); }
        }
    </style>
</head>
<body>

<div class="main-wrapper">
    <div class="glass-clock-container" id="clock-container">
        <div class="light-refractor" id="light-refractor"></div>
        <span id="date-text" class="date-label">جاري التحميل...</span>
        <h1 id="clock-text" class="iphone-glass-clock">00:00</h1>
    </div>

    <div class="widget-card flex justify-between items-center relative overflow-hidden">
        <div class="sun-rays"></div>
        <div class="z-10">
            <p class="text-white/40 text-[10px] font-black uppercase tracking-widest mb-1">الطقس الآن</p>
            <h3 class="text-white text-4xl font-black">٣٢°</h3>
            <p class="text-white/80 text-sm font-bold">جو كريستالي</p>
        </div>
        <i class="fas fa-sun text-5xl text-yellow-400 drop-shadow-[0_0_20px_rgba(250,204,21,0.8)] z-10"></i>
    </div>

    <div class="grid grid-cols-2 gap-4">
        <div class="widget-card flex flex-col justify-between aspect-square">
            <i class="fas fa-bolt text-cyan-400 text-3xl"></i>
            <div>
                <span class="text-white font-black text-2xl">٩٨٪</span>
                <p class="text-white/40 text-[10px] font-bold uppercase">البطارية</p>
            </div>
        </div>
        <div class="widget-card flex flex-col justify-between aspect-square">
            <i class="fas fa-heart text-pink-500 text-3xl"></i>
            <div>
                <span class="text-white font-black text-2xl">٧٢</span>
                <p class="text-white/40 text-[10px] font-bold uppercase">النبض</p>
            </div>
        </div>
    </div>

    <div class="dock">
        <div class="icon-box bg-gradient-to-b from-green-400 to-green-600"><i class="fas fa-phone"></i></div>
        <div class="icon-box bg-gradient-to-b from-blue-400 to-blue-600"><i class="fas fa-envelope"></i></div>
        <div class="icon-box bg-white"><i class="fab fa-safari text-blue-600 text-3xl"></i></div>
        <div class="icon-box bg-gradient-to-b from-purple-500 to-pink-500"><i class="fas fa-camera"></i></div>
    </div>
</div>

<script>
    function updateClock() {
        const now = new Date();
        const h = String(now.getHours()).padStart(2, '0');
        const m = String(now.getMinutes()).padStart(2, '0');
        document.getElementById('clock-text').innerText = `${h}:${m}`;
        const options = { weekday: 'long', day: 'numeric', month: 'long' };
        document.getElementById('date-text').innerText = now.toLocaleDateString('ar-SA', options);
    }
    
    setInterval(updateClock, 1000);
    updateClock();

    const container = document.getElementById('clock-container');
    const light = document.getElementById('light-refractor');

    // دعم اللمس للهواتف لعمل تأثير الميلان (Parallax)
    document.addEventListener('touchmove', (e) => {
        const touch = e.touches[0];
        const rect = container.getBoundingClientRect();
        const x = touch.clientX - rect.left;
        const y = touch.clientY - rect.top;

        light.style.opacity = '1';
        light.style.left = `${x - 200}px`;
        light.style.top = `${y - 200}px`;

        const rotateX = -(touch.clientY - window.innerHeight / 2) / 15;
        const rotateY = (touch.clientX - window.innerWidth / 2) / 15;
        container.style.transform = `rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
    }, { passive: true });

    document.addEventListener('touchend', () => {
        light.style.opacity = '0';
        container.style.transform = `rotateX(0deg) rotateY(0deg)`;
    });

    // للمتصفح العادي
    document.addEventListener('mousemove', (e) => {
        const rect = container.getBoundingClientRect();
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;
        light.style.opacity = '1';
        light.style.left = `${x - 200}px`;
        light.style.top = `${y - 200}px`;
        const rotateX = -(e.clientY - window.innerHeight / 2) / 20;
        const rotateY = (e.clientX - window.innerWidth / 2) / 20;
        container.style.transform = `rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
    });
</script>

</body>
</html>

