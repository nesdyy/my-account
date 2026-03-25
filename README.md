<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    Selamat Datang di Akun Nesdy
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            overflow-x: hidden;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            position: relative;
        }

        /* Particle Canvas */
        #particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            pointer-events: none;
        }

        /* Header */
        .header {
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            color: white;
            position: relative;
        }

        .typing-text {
            font-size: 3.5rem;
            font-weight: bold;
            margin-bottom: 20px;
            background: linear-gradient(45deg, #ff6b6b, #4ecdc4, #45b7d1, #f9ca24);
            background-size: 400% 400%;
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: gradientShift 3s ease infinite;
        }

        @keyframes gradientShift {
            0%, 100% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
        }

        .subtitle {
            font-size: 1.5rem;
            opacity: 0.9;
            margin-bottom: 40px;
        }

        /* Animated Buttons */
        .btn {
            padding: 15px 40px;
            font-size: 1.2rem;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            margin: 0 15px;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            font-weight: bold;
        }

        .btn-primary {
            background: linear-gradient(45deg, #ff6b6b, #ee5a24);
            color: white;
            box-shadow: 0 10px 30px rgba(255, 107, 107, 0.4);
        }

        .btn-secondary {
            background: linear-gradient(45deg, #4ecdc4, #44a08d);
            color: white;
            box-shadow: 0 10px 30px rgba(78, 205, 196, 0.4);
        }

        .btn:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
        }

        .btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
            transition: left 0.5s;
        }

        .btn:hover::before {
            left: 100%;
        }

        /* Sections */
        .section {
            padding: 100px 20px;
            max-width: 1200px;
            margin: 0 auto;
        }

        .section h2 {
            font-size: 3rem;
            text-align: center;
            margin-bottom: 50px;
            color: white;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .features {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
            margin-top: 50px;
        }

        .feature-card {
            background: rgba(255,255,255,0.1);
            backdrop-filter: blur(10px);
            padding: 40px;
            border-radius: 20px;
            text-align: center;
            color: white;
            transition: all 0.3s ease;
            border: 1px solid rgba(255,255,255,0.2);
        }

        .feature-card:hover {
            transform: translateY(-10px);
            background: rgba(255,255,255,0.2);
        }

        .feature-icon {
            font-size: 4rem;
            margin-bottom: 20px;
        }

        /* Scroll Indicator */
        .scroll-indicator {
            position: fixed;
            top: 20px;
            right: 20px;
            width: 8px;
            height: 8px;
            background: white;
            border-radius: 50%;
            transition: all 0.3s ease;
            z-index: 1000;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .typing-text {
                font-size: 2.5rem;
            }
            .subtitle {
                font-size: 1.2rem;
            }
        }
    </style>
</head>
<body>
    <!-- Particle Background -->
    <canvas id="particles"></canvas>

    <!-- Scroll Indicator -->
    <div class="scroll-indicator" id="scrollIndicator"></div>

    <!-- Header -->
    <header class="header">
        <h1 class="typing-text" id="typingText"></h1>
        <p class="subtitle">Buatan Nesdy yang Menakjubkan</p>
        <div>
            <button class="btn btn-primary" onclick="scrollToSection('features')">Lihat Fitur</button>
            <button class="btn btn-secondary" onclick="createConfetti()">🎉 Rayakan!</button>
        </div>
    </header>

    <!-- Features Section -->
    <section class="section" id="features">
        <h2>✨ Fitur Akun</h2>
        <div class="features">
            <div class="feature-card">
                <div class="feature-icon">🐱‍💻</div>
                <h3>Akun Nesdy</h3>
                <p>Transisi dan efek visual yang smooth menggunakan CSS3 dan JavaScript</p>
            </div>
            <div class="feature-card">
                <div class="feature-icon">⚡</div>
                <h3>Performa Tinggi</h3>
                <p>Optimasi canvas dan requestAnimationFrame untuk 60fps</p>
            </div>
            <div class="feature-card">
                <div class="feature-icon">📱</div>
                <h3>Responsive</h3>
                <p>Sempurna di semua device, dari mobile hingga desktop</p>
            </div>
        </div>
    </section>

    <script>
        // 1. PARTICLE SYSTEM
        class Particle {
            constructor(canvas) {
                this.canvas = canvas;
                this.ctx = canvas.getContext('2d');
                this.reset();
            }

            reset() {
                this.x = Math.random() * this.canvas.width;
                this.y = Math.random() * this.canvas.height;
                this.size = Math.random() * 3 + 1;
                this.speedX = Math.random() * 0.5 - 0.25;
                this.speedY = Math.random() * 0.5 - 0.25;
                this.opacity = Math.random() * 0.5 + 0.2;
            }

            update() {
                this.x += this.speedX;
                this.y += this.speedY;

                if (this.x < 0 || this.x > this.canvas.width) this.speedX *= -1;
                if (this.y < 0 || this.y > this.canvas.height) this.speedY *= -1;

                if (Math.random() < 0.01) this.reset();
            }

            draw() {
                this.ctx.save();
                this.ctx.globalAlpha = this.opacity;
                this.ctx.fillStyle = 'rgba(255, 255, 255, 0.8)';
                this.ctx.beginPath();
                this.ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                this.ctx.fill();
                this.ctx.restore();
            }
        }

        // Initialize Particles
        const canvas = document.getElementById('particles');
        const ctx = canvas.getContext('2d');
        let particles = [];
        let animationId;

        function initParticles() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            
            particles = [];
            for (let i = 0; i < 100; i++) {
                particles.push(new Particle(canvas));
            }
        }

        function animateParticles() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            particles.forEach(particle => {
                particle.update();
                particle.draw();
            });
            
            animationId = requestAnimationFrame(animateParticles);
        }

        // 2. TYPING EFFECT
        const textArray = [
            "Selamat Datang Di Akun Nesdy!",
            "HTML + CSS + JS",
            "Interactive Web",
            "Modern UI/UX"
        ];
        const typingElement = document.getElementById('typingText');
        let textIndex = 0;
        let charIndex = 0;
        let isDeleting = false;

        function typeWriter() {
            const currentText = textArray[textIndex];
            
            if (isDeleting) {
                typingElement.textContent = currentText.substring(0, charIndex - 1);
                charIndex--;
            } else {
                typingElement.textContent = currentText.substring(0, charIndex + 1);
                charIndex++;
            }

            let typeSpeed = isDeleting ? 50 : 100;
            
            if (!isDeleting && charIndex === currentText.length) {
                typeSpeed = 2000;
                isDeleting = true;
            } else if (isDeleting && charIndex === 0) {
                isDeleting = false;
                textIndex = (textIndex + 1) % textArray.length;
                typeSpeed = 500;
            }

            setTimeout(typeWriter, typeSpeed);
        }

        // 3. SMOOTH SCROLL
        function scrollToSection(sectionId) {
            document.getElementById(sectionId).scrollIntoView({
                behavior: 'smooth'
            });
        }

        // 4. CONFETTI EFFECT
        function createConfetti() {
            for (let i = 0; i < 100; i++) {
                const confetti = document.createElement('div');
                confetti.style.position = 'fixed';
                confetti.style.left = Math.random() * 100 + 'vw';
                confetti.style.top = '-10px';
                confetti.style.width = '10px';
                confetti.style.height = '10px';
                confetti.style.background = `hsl(${Math.random() * 360}, 70%, 60%)`;
                confetti.style.pointerEvents = 'none';
                confetti.style.zIndex = '1000';
                confetti.style.borderRadius = '50%';
                confetti.style.animation = `fall ${Math.random() * 3 + 2}s linear forwards`;
                document.body.appendChild(confetti);

                setTimeout(() => confetti.remove(), 5000);
            }
        }

        // Add CSS for confetti
        const style = document.createElement('style');
        style.textContent = `
            @keyframes fall {
                to {
                    transform: translateY(100vh) rotate(720deg);
                    opacity: 0;
                }
            }
        `;
        document.head.appendChild(style);

        // 5. SCROLL INDICATOR
        window.addEventListener('scroll', () => {
            const scrollIndicator = document.getElementById('scrollIndicator');
            const scrollPercent = (window.scrollY / (document.body.scrollHeight - window.innerHeight)) * 100;
            scrollIndicator.style.transform = `scale(${1 + scrollPercent / 100})`;
            scrollIndicator.style.opacity = scrollPercent / 100;
        });

        // 6. INTERSECTION OBSERVER untuk animasi
        const observerOptions = {
            threshold: 0.1,
            rootMargin: '0px 0px -50px 0px'
        };

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.style.opacity = '1';
                    entry.target.style.transform = 'translateY(0)';
                }
            });
        }, observerOptions);

        // Observe feature cards
        document.querySelectorAll('.feature-card').forEach(card => {
            card.style.opacity = '0';
            card.style.transform = 'translateY(50px)';
            card.style.transition = 'all 0.6s ease';
            observer.observe(card);
        });

        // 7. INITIALIZE
        window.addEventListener('load', () => {
            initParticles();
            animateParticles();
            typeWriter();
        });

        window.addEventListener('resize', initParticles);

        // Pause animation when tab is not active
        document.addEventListener('visibilitychange', () => {
            if (document.hidden) {
                cancelAnimationFrame(animationId);
            } else {
                animateParticles();
            }
        });
    </script>
</body>
</html>
