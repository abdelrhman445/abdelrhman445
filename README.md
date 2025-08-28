<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NullSpecter - Red Team Operative</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&family=Share+Tech+Mono&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --main-bg: #0d1117;
            --terminal-bg: #0a0e14;
            --accent-primary: #ff2a6d;
            --accent-secondary: #05d9e8;
            --accent-tertiary: #b388ff;
            --text-primary: #f0f6fc;
            --text-secondary: #8b9bb4;
            --terminal-border: #1c2333;
            --progress-fill: var(--accent-primary);
            --progress-bg: #1c2333;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            background-color: var(--main-bg);
            color: var(--text-primary);
            font-family: 'Cairo', sans-serif;
            line-height: 1.8;
            padding: 20px;
            overflow-x: hidden;
            background-image: 
                radial-gradient(circle at 15% 50%, rgba(35, 25, 70, 0.2) 0%, transparent 25%),
                radial-gradient(circle at 85% 30%, rgba(70, 25, 70, 0.2) 0%, transparent 25%);
        }
        
        .matrix-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            opacity: 0.1;
            pointer-events: none;
        }
        
        .container {
            max-width: 1000px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }
        
        header {
            text-align: center;
            padding: 30px 20px;
            position: relative;
            border-bottom: 1px solid var(--terminal-border);
            margin-bottom: 30px;
        }
        
        h1 {
            font-family: 'Cairo', sans-serif;
            font-size: 3.2rem;
            margin-bottom: 10px;
            color: var(--accent-primary);
            text-shadow: 0 0 10px rgba(255, 42, 109, 0.5);
            letter-spacing: 2px;
        }
        
        h2 {
            font-family: 'Cairo', sans-serif;
            color: var(--accent-secondary);
            margin: 25px 0 15px;
            font-size: 1.8rem;
            border-right: 4px solid var(--accent-primary);
            padding-right: 15px;
            text-align: right;
        }
        
        h3 {
            color: var(--accent-tertiary);
            font-family: 'Share Tech Mono', monospace;
            font-size: 1.4rem;
            margin-bottom: 20px;
        }
        
        p {
            margin-bottom: 20px;
            color: var(--text-secondary);
            text-align: right;
        }
        
        .social-links {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin: 20px 0;
        }
        
        .social-links a {
            color: var(--text-primary);
            font-size: 1.5rem;
            transition: all 0.3s ease;
        }
        
        .social-links a:hover {
            color: var(--accent-primary);
            transform: translateY(-3px);
        }
        
        .contact-info {
            margin: 15px 0;
            font-size: 1.1rem;
        }
        
        .terminal {
            background-color: var(--terminal-bg);
            border: 1px solid var(--terminal-border);
            border-radius: 10px;
            padding: 20px;
            margin: 25px 0;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
            position: relative;
            overflow: hidden;
            text-align: right;
        }
        
        .terminal::before {
            content: '';
            position: absolute;
            top: 0;
            right: 0;
            left: 0;
            height: 1px;
            background: linear-gradient(90deg, 
                transparent, 
                var(--accent-secondary), 
                transparent);
            animation: scanline 3s linear infinite;
        }
        
        @keyframes scanline {
            0% { top: 0; }
            100% { top: 100%; }
        }
        
        .skill-bar {
            margin-bottom: 15px;
        }
        
        .skill-name {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
            font-size: 0.9rem;
        }
        
        .skill-progress {
            height: 10px;
            background-color: var(--progress-bg);
            border-radius: 5px;
            overflow: hidden;
            position: relative;
        }
        
        .skill-progress-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--accent-primary), var(--accent-tertiary));
            border-radius: 5px;
            width: 0;
            transition: width 1.5s ease-in-out;
        }
        
        .projects-list {
            list-style: none;
        }
        
        .projects-list li {
            margin-bottom: 15px;
            padding-right: 20px;
            position: relative;
            text-align: right;
        }
        
        .projects-list li::before {
            content: "➔";
            color: var(--accent-primary);
            position: absolute;
            right: 0;
        }
        
        .stats-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin: 25px 0;
        }
        
        .stat-box {
            background-color: var(--terminal-bg);
            border: 1px solid var(--terminal-border);
            border-radius: 8px;
            padding: 15px;
            text-align: center;
            transition: transform 0.3s ease;
        }
        
        .stat-box:hover {
            transform: translateY(-5px);
            border-color: var(--accent-secondary);
        }
        
        .stat-value {
            font-size: 2rem;
            font-weight: bold;
            color: var(--accent-primary);
            margin: 10px 0;
        }
        
        .stat-label {
            color: var(--text-secondary);
            font-size: 0.9rem;
        }
        
        .glitch-container {
            position: relative;
            display: inline-block;
        }
        
        .glitch {
            position: relative;
            display: inline-block;
        }
        
        .glitch::before, .glitch::after {
            content: attr(data-text);
            position: absolute;
            top: 0;
            right: 0;
            width: 100%;
            height: 100%;
        }
        
        .glitch::before {
            animation: glitch-effect 3s infinite;
            color: var(--accent-primary);
            right: 2px;
            text-shadow: -2px 0 var(--accent-secondary);
            clip: rect(44px, 450px, 56px, 0);
            opacity: 0.8;
        }
        
        .glitch::after {
            animation: glitch-effect 2s infinite;
            color: var(--accent-secondary);
            right: -2px;
            text-shadow: -2px 0 var(--accent-primary);
            clip: rect(44px, 450px, 56px, 0);
            opacity: 0.8;
        }
        
        @keyframes glitch-effect {
            0% { clip: rect(42px, 9999px, 59px, 0); }
            5% { clip: rect(2px, 9999px, 78px, 0); }
            10% { clip: rect(60px, 9999px, 75px, 0); }
            15% { clip: rect(64px, 9999px, 30px, 0); }
            20% { clip: rect(64px, 9999px, 32px, 0); }
            25% { clip: rect(2px, 9999px, 7px, 0); }
            30% { clip: rect(26px, 9999px, 78px, 0); }
            35% { clip: rect(72px, 9999px, 55px, 0); }
            40% { clip: rect(9px, 9999px, 30px, 0); }
            45% { clip: rect(85px, 9999px, 30px, 0); }
            50% { clip: rect(85px, 9999px, 25px, 0); }
            55% { clip: rect(85px, 9999px, 55px, 0); }
            60% { clip: rect(18px, 9999px, 55px, 0); }
            65% { clip: rect(30px, 9999px, 72px, 0); }
            70% { clip: rect(20px, 9999px, 18px, 0); }
            75% { clip: rect(60px, 9999px, 70px, 0); }
            80% { clip: rect(10px, 9999px, 35px, 0); }
            85% { clip: rect(35px, 9999px, 15px, 0); }
            90% { clip: rect(30px, 9999px, 60px, 0); }
            95% { clip: rect(85px, 9999px, 10px, 0); }
            100% { clip: rect(75px, 9999px, 95px, 0); }
        }
        
        .hacker-text {
            font-family: 'Share Tech Mono', monospace;
            color: var(--accent-secondary);
            text-shadow: 0 0 5px var(--accent-secondary);
            text-align: right;
        }
        
        .typewriter {
            overflow: hidden;
            border-left: 0.15em solid var(--accent-primary);
            white-space: nowrap;
            margin: 0 auto;
            letter-spacing: 0.15em;
            animation: typing 3.5s steps(40, end), blink-caret 0.75s step-end infinite;
        }
        
        @keyframes typing {
            from { width: 0; }
            to { width: 100%; }
        }
        
        @keyframes blink-caret {
            from, to { border-color: transparent; }
            50% { border-color: var(--accent-primary); }
        }
        
        footer {
            text-align: center;
            margin-top: 50px;
            padding: 20px;
            border-top: 1px solid var(--terminal-border);
            color: var(--text-secondary);
            font-size: 0.9rem;
        }
        
        .cyber-button {
            display: inline-block;
            padding: 10px 20px;
            margin: 10px 5px;
            background: transparent;
            color: var(--accent-primary);
            border: 1px solid var(--accent-primary);
            border-radius: 4px;
            font-family: 'Share Tech Mono', monospace;
            text-transform: uppercase;
            letter-spacing: 2px;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .cyber-button:hover {
            background: var(--accent-primary);
            color: var(--main-bg);
            box-shadow: 0 0 10px var(--accent-primary);
        }
        
        .cyber-button::before {
            content: '';
            position: absolute;
            top: 0;
            right: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            transition: 0.5s;
        }
        
        .cyber-button:hover::before {
            right: 100%;
        }
        
        @media (max-width: 768px) {
            h1 {
                font-size: 2.2rem;
            }
            
            h2 {
                font-size: 1.5rem;
            }
            
            .stats-container {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <canvas class="matrix-bg" id="matrixCanvas"></canvas>
    
    <div class="container">
        <header>
            <h1 class="glitch" data-text="NullSpecter">NullSpecter</h1>
            <h3 class="typewriter">"الهجوم بالتصميم" · مشغل ثنائي الوضع · مُولد حمولة خفية</h3>
            
            <div class="social-links">
                <a href="https://www.linkedin.com/in/abdulrahman-elsayed-59a664313" target="_blank"><i class="fab fa-linkedin"></i></a>
                <a href="https://www.youtube.com/@gamotek175" target="_blank"><i class="fab fa-youtube"></i></a>
                <a href="https://www.facebook.com/abdulelsayd" target="_blank"><i class="fab fa-facebook"></i></a>
                <a href="https://github.com/NullSpecter" target="_blank"><i class="fab fa-github"></i></a>
            </div>
            
            <div class="contact-info">
                <p><i class="fas fa-envelope"></i> <strong>boodapro540@gmail.com</strong> | <i class="fas fa-phone"></i> <strong>+20 101 185 7244</strong></p>
            </div>
            
            <div class="cyber-buttons">
                <button class="cyber-button">تحميل السيرة الذاتية</button>
                <button class="cyber-button">اتصل بي</button>
                <button class="cyber-button">مشاريعي</button>
            </div>
        </header>
        
        <section>
            <h2>🚀 نبذة عن المهمة</h2>
            <div class="terminal">
                <p class="hacker-text">> تهيئة معلمات المهمة...</p>
                <p class="hacker-text">> تحميل البروتوكولات التكتيكية...</p>
                <p>
                    🧠 متخصص في فريق الأحمر يجمع بين الاختراقات التكتيكية ومحاكاة الثغرات يوم الصفر والأدوات المخصصة.<br>
                    🔍 مهندس بيئات اختبار واقعية لأسطح الهجوم على مستوى المؤسسات.<br>
                    🛠️ مبتكر <strong style="color:var(--accent-primary);">Rebel Security Scanner v4.0</strong> — يستهدف: XSS و SQLi و LFI و SSRF وثغرات التحميل وتجاوز الرأس والمزيد.<br>
                    🎯 يركز على التهرب من الدفاعات وتسليح التعليمات البرمجية واختراق البنية التحتية.
                </p>
                <p class="hacker-text">> اكتمال الإحاطة بالمهمة. جاهز للاشتباك.</p>
            </div>
        </section>
        
        <section>
            <h2>⚔️ كفاءة الأسلحة</h2>
            <div class="terminal">
                <div class="skill-bar">
                    <div class="skill-name">
                        <span>Python</span>
                        <span>85%</span>
                    </div>
                    <div class="skill-progress">
                        <div class="skill-progress-fill" data-width="85%"></div>
                    </div>
                </div>
                
                <div class="skill-bar">
                    <div class="skill-name">
                        <span>JavaScript</span>
                        <span>75%</span>
                    </div>
                    <div class="skill-progress">
                        <div class="skill-progress-fill" data-width="75%"></div>
                    </div>
                </div>
                
                <div class="skill-bar">
                    <div class="skill-name">
                        <span>HTML/CSS</span>
                        <span>80%</span>
                    </div>
                    <div class="skill-progress">
                        <div class="skill-progress-fill" data-width="80%"></div>
                    </div>
                </div>
                
                <div class="skill-bar">
                    <div class="skill-name">
                        <span>PHP</span>
                        <span>70%</span>
                    </div>
                    <div class="skill-progress">
                        <div class="skill-progress-fill" data-width="70%"></div>
                    </div>
                </div>
                
                <div class="skill-bar">
                    <div class="skill-name">
                        <span>C/C++</span>
                        <span>65%</span>
                    </div>
                    <div class="skill-progress">
                        <div class="skill-progress-fill" data-width="65%"></div>
                    </div>
                </div>
                
                <div class="skill-bar">
                    <div class="skill-name">
                        <span>Bash</span>
                        <span>80%</span>
                    </div>
                    <div class="skill-progress">
                        <div class="skill-progress-fill" data-width="80%"></div>
                    </div>
                </div>
            </div>
        </section>
        
        <section>
            <h2>🔥 المشاريع المميزة</h2>
            <div class="terminal">
                <ul class="projects-list">
                    <li>🧨 <strong style="color:var(--accent-primary);">Rebel Security Scanner v4.0</strong> — CLI + HTML مجموعة أدوات استغلال كاملة</li>
                    <li>💬 <strong style="color:var(--accent-primary);">ChatOps Intelligence Hub</strong> — نظام دردشة آمن في الوقت الفعلي باستخدام Socket.io</li>
                    <li>📺 <strong style="color:var(--accent-primary);">YouTube Course Manager</strong> — لوحة full-stack مبنية بـ React و Node.js و MongoDB</li>
                    <li>🔐 <strong style="color:var(--accent-primary);">Keylogger خفي</strong> — آلية ثبات غير قابلة للكشف مع C2 مشفر</li>
                    <li>🌐 <strong style="color:var(--accent-primary);">إطار التصيد</strong> — محرك قوالب متقدم مع التتبع وتحليلات</li>
                </ul>
            </div>
        </section>
        
        <section>
            <h2>📈 إحصائيات المعارك</h2>
            <div class="stats-container">
                <div class="stat-box">
                    <div class="stat-value">150+</div>
                    <div class="stat-label">ثغرة تم اكتشافها</div>
                </div>
                <div class="stat-box">
                    <div class="stat-value">42</div>
                    <div class="stat-label">مشروع مكتمل</div>
                </div>
                <div class="stat-box">
                    <div class="stat-value">27</div>
                    <div class="stat-label">أداة مطورة</div>
                </div>
            </div>
            
            <div class="terminal">
                <p class="hacker-text">> تحميل إحصائيات GitHub...</p>
                <div class="github-stats">
                    <!-- Placeholder for GitHub stats -->
                    <p style="text-align: center; padding: 20px;">
                        <i class="fas fa-code-branch" style="color: var(--accent-secondary); font-size: 2rem;"></i>
                        <br>
                        <span class="hacker-text">تفعيل تكامل مقاييس GitHub</span>
                    </p>
                </div>
            </div>
        </section>
        
        <section>
            <h2>🎓 ساحة التدريب على الأمن</h2>
            <div class="terminal">
                <p>
                    🏗️ محاكاة سيناريوهات CTF في العالم الحقيقي ومنطق الحمولة.<br>
                    🧬 شغوف بالاختبار الغامض وما بعد الاستغلال ونصوص فريق الأحمر.<br>
                    📢 مشاركة المعرفة الهجومية من خلال الأدوات ومقاطع الفيديو و GitHub.
                </p>
                <p class="hacker-text">> التدريب المستمر: اختبار الاختراق المتقدم</p>
                <p class="hacker-text">> الشهادة التالية: OSCP</p>
            </div>
        </section>
        
        <footer>
            <p>NullSpecter © 2023 | operative فريق الأحمر | "الهجوم بالتصميم"</p>
            <p class="hacker-text">النظام آمن. لم يتم اكتشاف أي شذوذ.</p>
        </footer>
    </div>

    <script>
        // Matrix background effect
        const canvas = document.getElementById('matrixCanvas');
        const ctx = canvas.getContext('2d');
        
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
        
        const letters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789$#@%&!?*^(){}[]<>';
        const fontSize = 12;
        const columns = canvas.width / fontSize;
        
        const drops = [];
        for (let i = 0; i < columns; i++) {
            drops[i] = 1;
        }
        
        function drawMatrix() {
            ctx.fillStyle = 'rgba(13, 17, 23, 0.1)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            ctx.fillStyle = '#05d9e8';
            ctx.font = `${fontSize}px 'Share Tech Mono'`;
            
            for (let i = 0; i < drops.length; i++) {
                const text = letters[Math.floor(Math.random() * letters.length)];
                ctx.fillText(text, i * fontSize, drops[i] * fontSize);
                
                if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
                    drops[i] = 0;
                }
                
                drops[i]++;
            }
            
            requestAnimationFrame(drawMatrix);
        }
        
        drawMatrix();
        
        // Animate skill bars on scroll
        function animateSkillBars() {
            const skillBars = document.querySelectorAll('.skill-progress-fill');
            skillBars.forEach(bar => {
                const width = bar.getAttribute('data-width');
                bar.style.width = width;
            });
        }
        
        // Initialize animations when page loads
        window.addEventListener('load', function() {
            animateSkillBars();
            
            // Add glitch effect randomly to headers
            setInterval(() => {
                const glitch = document.querySelector('.glitch');
                glitch.classList.toggle('glitch-effect');
            }, 5000);
        });
        
        // Responsive canvas
        window.addEventListener('resize', function() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        });
    </script>
</body>
</html>
