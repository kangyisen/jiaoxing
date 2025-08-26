<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>新同学</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: #000;
            overflow: hidden;
            font-family: 'Arial', sans-serif;
        }
        
        .container {
            position: relative;
            width: 100%;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .heart {
            position: relative;
            width: 200px;
            height: 200px;
            background: #ff4757;
            transform: rotate(45deg);
            box-shadow: 
                0 0 50px #ff4757,
                0 0 100px #ff4757;
            animation: heartbeat 1.2s infinite;
            z-index: 10;
        }
        
        .heart::before,
        .heart::after {
            content: '';
            position: absolute;
            width: 200px;
            height: 200px;
            background: #ff4757;
            border-radius: 50%;
        }
        
        .heart::before {
            top: -100px;
            left: 0;
        }
        
        .heart::after {
            top: 0;
            left: -100px;
        }
        
        /* 心跳动画 */
        @keyframes heartbeat {
            0% {
                transform: rotate(45deg) scale(1);
            }
            25% {
                transform: rotate(45deg) scale(1.1);
            }
            50% {
                transform: rotate(45deg) scale(1);
            }
            75% {
                transform: rotate(45deg) scale(1.2);
            }
            100% {
                transform: rotate(45deg) scale(1);
            }
        }
        
        /* 粒子样式 */
        .particles {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            z-index: 1;
        }
        
        .particle {
            position: absolute;
            width: 6px;
            height: 6px;
            background: #ff4757;
            border-radius: 50%;
            box-shadow: 0 0 10px #ff4757, 0 0 20px #ff4757;
            animation: float 5s infinite ease-out;
        }
        
        @keyframes float {
            0% {
                opacity: 1;
                transform: translate(0, 0) scale(1);
            }
            100% {
                opacity: 0;
                transform: translate(var(--tx), var(--ty)) scale(0.1);
            }
        }
        
        .title {
            position: absolute;
            top: 20px;
            color: rgba(255, 255, 255, 0.7);
            font-size: 24px;
            text-shadow: 0 0 10px rgba(255, 71, 87, 0.8);
            letter-spacing: 2px;
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { opacity: 0.7; }
            50% { opacity: 1; }
            100% { opacity: 0.7; }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1 class="title">邱梦晗大笨蛋</h1>
        <div class="heart"></div>
        <div class="particles" id="particles"></div>
    </div>

    <script>
        // 创建粒子
        function createParticles() {
            const particlesContainer = document.getElementById('particles');
            const heart = document.querySelector('.heart');
            
            // 获取爱心位置
            const heartRect = heart.getBoundingClientRect();
            const heartX = heartRect.left + heartRect.width / 2;
            const heartY = heartRect.top + heartRect.height / 2;
            
            // 创建粒子数量
            const particleCount = 80;
            
            for (let i = 0; i < particleCount; i++) {
                const particle = document.createElement('div');
                particle.classList.add('particle');
                
                // 随机位置在爱心周围
                const angle = Math.random() * Math.PI * 2;
                const radius = Math.random() * 150 + 50;
                const x = heartX + Math.cos(angle) * radius;
                const y = heartY + Math.sin(angle) * radius;
                
                particle.style.left = `${x}px`;
                particle.style.top = `${y}px`;
                
                // 随机目标位置
                const tx = (Math.random() - 0.5) * 1000;
                const ty = (Math.random() - 0.5) * 1000;
                
                particle.style.setProperty('--tx', `${tx}px`);
                particle.style.setProperty('--ty', `${ty}px`);
                
                // 随机动画延迟和持续时间
                const delay = Math.random() * 5;
                const duration = Math.random() * 5 + 5;
                
                particle.style.animationDelay = `${delay}s`;
                particle.style.animationDuration = `${duration}s`;
                
                // 随机大小
                const size = Math.random() * 4 + 2;
                particle.style.width = `${size}px`;
                particle.style.height = `${size}px`;
                
                // 随机颜色变化
                const hue = 340 + Math.random() * 20;
                const color = `hsl(${hue}, 90%, 60%)`;
                particle.style.background = color;
                particle.style.boxShadow = `0 0 10px ${color}, 0 0 20px ${color}`;
                
                particlesContainer.appendChild(particle);
                
                // 动画结束后移除粒子
                setTimeout(() => {
                    particle.remove();
                }, (delay + duration) * 1000);
            }
        }
        
        // 初始创建粒子
        createParticles();
        
        // 定期创建新粒子
        setInterval(() => {
            createParticles();
        }, 500);
        
        // 窗口调整大小时重新定位粒子
        window.addEventListener('resize', () => {
            document.getElementById('particles').innerHTML = '';
            createParticles();
        });
    </script>
</body>
</html>
