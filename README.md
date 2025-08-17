<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>apha zyat intro</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@600;800&display=swap" rel="stylesheet">
  <style>
    body {
      margin: 0;
      overflow: hidden;
      background-color: #222;
      background-image: url("https://i.supaimg.com/142d1042-e346-4176-8237-fa26a3bc6c2c.jpg");
      background-size: cover;
      background-position: center;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    /* ===== INTRO SCREEN ===== */
    #intro {
      position: fixed;
      top: 0; left: 0;
      width: 100%;
      height: 100%;
      background: #000;
      display: flex;
      justify-content: center;
      align-items: center;
      text-align: center;
      flex-direction: column;
      padding: 20px;
      z-index: 9999;
      animation: fadeOut 2s ease 5s forwards; /* keluar setelah 5 detik */
    }

    #intro h1 {
      color: #00ff88; /* hijau neon */
      font-size: 2em;
      font-family: 'Orbitron', sans-serif;
      line-height: 1.5;
      letter-spacing: 2px;
      opacity: 0;
      animation: fadeInScale 3s ease forwards;
      text-shadow: 0 0 10px #00ff88, 0 0 20px #00ff88;
    }

    @keyframes fadeInScale {
      0% { opacity: 0; transform: scale(0.5); }
      100% { opacity: 1; transform: scale(1); }
    }

    @keyframes fadeOut {
      to { opacity: 0; visibility: hidden; }
    }

    /* ===== MAIN CONTENT ===== */
    #anime-container {
      position: relative;
      width: 400px;
      height: 400px;
      overflow: hidden;
      border-radius: 20px;
      box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
    }

    #anime-image {
      width: 100%;
      height: 100%;
      object-fit: cover;
      animation: pan 15s linear infinite;
    }

    @keyframes pan {
      0% { transform: scale(1.2) translate(-10%, -10%); }
      50% { transform: scale(1.2) translate(10%, 10%); }
      100% { transform: scale(1.2) translate(-10%, -10%); }
    }

    #overlay {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.3);
      mix-blend-mode: overlay;
    }

    #particles {
      position: absolute;
      width: 100%;
      height: 100%;
      pointer-events: none;
    }

    #stars-canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 1;
    }
  </style>
</head>
<body>
  <!-- INTRO SCREEN -->
  <div id="intro">
    <h1>✨ "Tubuhku mungkin mesin, tapi tekadku tetap manusia. <br> Aku adalah Alpha, prajurit yang takkan pernah mundur." ✨</h1>
  </div>

  <!-- STAR EFFECT FULLSCREEN -->
  <canvas id="stars-canvas"></canvas>

  <!-- MAIN ANIME CONTAINER -->
  <div id="anime-container">
    <img id="anime-image" src="https://i.supaimg.com/142d1042-e346-4176-8237-fa26a3bc6c2c.jpg" alt="Anime Image">
    <div id="overlay"></div>
    <canvas id="particles"></canvas>
  </div>

  <script>
    // ========== Partikel bintang kecil di dalam kotak ==========
    const canvas = document.getElementById('particles');
    const ctx = canvas.getContext('2d');
    canvas.width = 400;
    canvas.height = 400;

    const particlesArray = [];
    const numberOfParticles = 100;

    class Particle {
      constructor(){
        this.x = Math.random() * canvas.width;
        this.y = Math.random() * canvas.height;
        this.size = Math.random() * 3 + 1;
        this.speedX = Math.random() * 1 - 0.5;
        this.speedY = Math.random() * 1 - 0.5;
        this.color = 'rgba(255, 255, 255, 0.2)';
      }
      update(){
        this.x += this.speedX;
        this.y += this.speedY;
        if (this.x < 0 || this.x > canvas.width) this.speedX = -this.speedX;
        if (this.y < 0 || this.y > canvas.height) this.speedY = -this.speedY;
      }
      draw(){
        ctx.fillStyle = this.color;
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
        ctx.closePath();
        ctx.fill();
      }
    }

    function init(){
      for (let i = 0; i < numberOfParticles; i++){
        particlesArray.push(new Particle());
      }
    }

    function animate(){
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      for (let i = 0; i < particlesArray.length; i++){
        particlesArray[i].update();
        particlesArray[i].draw();
      }
      requestAnimationFrame(animate);
    }

    init();
    animate();

    // ========== Efek Bintang Fullscreen ==========
    const starCanvas = document.getElementById('stars-canvas');
    const starCtx = starCanvas.getContext('2d');
    starCanvas.width = window.innerWidth;
    starCanvas.height = window.innerHeight;

    const stars = [];

    class Star {
      constructor(){
        this.x = Math.random() * starCanvas.width;
        this.y = Math.random() * starCanvas.height;
        this.size = Math.random() * 2;
        this.speedY = Math.random() * 0.5 + 0.2;
        this.alpha = Math.random();
      }
      draw(){
        starCtx.fillStyle = `rgba(255, 255, 255, ${this.alpha})`;
        starCtx.beginPath();
        starCtx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
        starCtx.fill();
      }
      update(){
        this.y += this.speedY;
        if(this.y > starCanvas.height){
          this.y = 0;
          this.x = Math.random() * starCanvas.width;
        }
        this.alpha += (Math.random() - 0.5) * 0.05;
        if(this.alpha < 0) this.alpha = 0;
        if(this.alpha > 1) this.alpha = 1;
      }
    }

    function initStars(){
      for(let i = 0; i < 200; i++){
        stars.push(new Star());
      }
    }

    function animateStars(){
      starCtx.clearRect(0, 0, starCanvas.width, starCanvas.height);
      for(let i = 0; i < stars.length; i++){
        stars[i].update();
        stars[i].draw();
      }
      requestAnimationFrame(animateStars);
    }

    initStars();
    animateStars();

    window.addEventListener("resize", () => {
      starCanvas.width = window.innerWidth;
      starCanvas.height = window.innerHeight;
    });
  </script>
</body>
</html>
