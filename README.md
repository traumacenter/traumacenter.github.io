<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Trauma Center - 간호사를 위한 외상 교육 센터</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: 'Arial', 'Noto Sans KR', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }
        
        /* 헤더 스타일 */
        .header {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            box-shadow: 0 2px 20px rgba(0,0,0,0.1);
            position: sticky;
            top: 0;
            z-index: 1000;
            padding: 15px 0;
        }
        
        .header-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
            text-decoration: none;
            color: #2c3e50;
        }
        
        .logo-icon {
            font-size: clamp(24px, 4vw, 32px);
        }
        
        .logo-text {
            font-size: clamp(18px, 3.5vw, 24px);
            font-weight: bold;
        }
        
        .nav-menu {
            display: flex;
            list-style: none;
            gap: 20px;
            align-items: center;
        }
        
        .nav-item {
            position: relative;
        }
        
        .nav-link {
            text-decoration: none;
            color: #2c3e50;
            font-weight: 600;
            font-size: clamp(14px, 2.5vw, 16px);
            padding: 10px 15px;
            border-radius: 8px;
            transition: all 0.3s ease;
            display: block;
        }
        
        .nav-link:hover {
            background: rgba(52, 152, 219, 0.1);
            color: #3498db;
        }
        
        /* 모바일 메뉴 토글 */
        .menu-toggle {
            display: none;
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            color: #2c3e50;
        }
        
        /* 모바일 반응형 */
        @media (max-width: 768px) {
            .header-container {
                padding: 0 15px;
            }
            
            .menu-toggle {
                display: block;
            }
            
            .nav-menu {
                position: absolute;
                top: 100%;
                left: 0;
                right: 0;
                background: rgba(255, 255, 255, 0.98);
                backdrop-filter: blur(10px);
                flex-direction: column;
                gap: 0;
                box-shadow: 0 5px 20px rgba(0,0,0,0.1);
                transform: translateY(-10px);
                opacity: 0;
                visibility: hidden;
                transition: all 0.3s ease;
            }
            
            .nav-menu.active {
                transform: translateY(0);
                opacity: 1;
                visibility: visible;
            }
            
            .nav-link {
                padding: 15px 20px;
                border-bottom: 1px solid rgba(0,0,0,0.1);
            }
            
            .nav-link:last-child {
                border-bottom: none;
            }
        }
        
        /* 메인 컨테이너 */
        .main-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 40px 20px;
        }
        
        /* 히어로 섹션 */
        .hero-section {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: clamp(30px, 6vw, 60px);
            text-align: center;
            margin-bottom: 40px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.1);
        }
        
        .hero-title {
            font-size: clamp(2em, 5vw, 3.5em);
            color: #2c3e50;
            margin-bottom: 20px;
            line-height: 1.2;
        }
        
        .hero-subtitle {
            font-size: clamp(1.1em, 2.5vw, 1.4em);
            color: #7f8c8d;
            margin-bottom: 30px;
            line-height: 1.5;
        }
        
        .hero-description {
            font-size: clamp(1em, 2.2vw, 1.1em);
            color: #555;
            max-width: 800px;
            margin: 0 auto 40px;
            line-height: 1.6;
        }
        
        /* 카드 그리드 */
        .cards-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 30px;
            margin-top: 40px;
        }
        
        @media (max-width: 768px) {
            .cards-grid {
                grid-template-columns: 1fr;
                gap: 20px;
            }
        }
        
        /* 카드 스타일 */
        .learning-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.1);
            transition: all 0.3s ease;
            cursor: pointer;
            text-decoration: none;
            color: inherit;
            border: 2px solid transparent;
        }
        
        .learning-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 50px rgba(0,0,0,0.15);
            border-color: #3498db;
        }
        
        .card-icon {
            font-size: clamp(3em, 6vw, 4em);
            margin-bottom: 20px;
            display: block;
        }
        
        .card-title {
            font-size: clamp(1.3em, 3vw, 1.6em);
            color: #2c3e50;
            margin-bottom: 15px;
            font-weight: bold;
            line-height: 1.3;
        }
        
        .card-description {
            font-size: clamp(0.95em, 2.2vw, 1.05em);
            color: #666;
            line-height: 1.6;
            margin-bottom: 20px;
        }
        
        .card-features {
            list-style: none;
            margin-bottom: 25px;
        }
        
        .card-features li {
            font-size: clamp(0.9em, 2vw, 1em);
            color: #555;
            margin-bottom: 8px;
            padding-left: 20px;
            position: relative;
        }
        
        .card-features li:before {
            content: "✓";
            position: absolute;
            left: 0;
            color: #27ae60;
            font-weight: bold;
        }
        
        .card-button {
            background: linear-gradient(135deg, #3498db, #2980b9);
            color: white;
            padding: 12px 25px;
            border-radius: 25px;
            font-weight: bold;
            font-size: clamp(0.9em, 2vw, 1em);
            border: none;
            cursor: pointer;
            transition: all 0.3s ease;
            width: 100%;
        }
        
        .card-button:hover {
            background: linear-gradient(135deg, #2980b9, #1f5f7e);
            transform: translateY(-2px);
        }
        
        /* 특색 카드 */
        .card-trauma-mechanisms {
            border-left: 5px solid #e74c3c;
        }
        
        .card-traffic-accidents {
            border-left: 5px solid #f39c12;
        }
        
        /* 푸터 */
        .footer {
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(10px);
            margin-top: 60px;
            padding: 30px 20px;
            text-align: center;
            border-radius: 20px 20px 0 0;
        }
        
        .footer-content {
            max-width: 1200px;
            margin: 0 auto;
            color: #7f8c8d;
            font-size: clamp(0.9em, 2vw, 1em);
            line-height: 1.6;
        }
        
        .footer-title {
            color: #2c3e50;
            font-weight: bold;
            margin-bottom: 10px;
            font-size: clamp(1.1em, 2.5vw, 1.3em);
        }
        
        /* 애니메이션 */
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        .hero-section {
            animation: fadeInUp 0.8s ease-out;
        }
        
        .learning-card {
            animation: fadeInUp 0.8s ease-out;
            animation-fill-mode: both;
        }
        
        .learning-card:nth-child(1) {
            animation-delay: 0.2s;
        }
        
        .learning-card:nth-child(2) {
            animation-delay: 0.4s;
        }
        
        /* 접근성 */
        @media (prefers-reduced-motion: reduce) {
            * {
                animation-duration: 0.01ms !important;
                animation-iteration-count: 1 !important;
                transition-duration: 0.01ms !important;
            }
        }
        
        /* 고해상도 디스플레이 */
        @media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
            .hero-section,
            .learning-card,
            .footer {
                backdrop-filter: blur(15px);
            }
        }
    </style>
</head>
<body>
    <!-- 헤더 -->
    <header class="header">
        <div class="header-container">
            <a href="#" class="logo">
                <span class="logo-icon">🏥</span>
                <span class="logo-text">Trauma Center</span>
            </a>
            
            <nav>
                <button class="menu-toggle" onclick="toggleMenu()">☰</button>
                <ul class="nav-menu" id="nav-menu">
                    <li class="nav-item"><a href="#" class="nav-link">홈</a></li>
                    <li class="nav-item"><a href="#trauma-mechanisms" class="nav-link">외상기전</a></li>
                    <li class="nav-item"><a href="#traffic-accidents" class="nav-link">교통사고</a></li>
                    <li class="nav-item"><a href="#about" class="nav-link">소개</a></li>
                    <li class="nav-item"><a href="#contact" class="nav-link">문의</a></li>
                </ul>
            </nav>
        </div>
    </header>

    <!-- 메인 컨테이너 -->
    <div class="main-container">
        <!-- 히어로 섹션 -->
        <section class="hero-section">
            <h1 class="hero-title">🏥 Trauma Center</h1>
            <p class="hero-subtitle">학생간호사 및 신규간호사를 위한 외상 교육 센터</p>
            <p class="hero-description">
                체계적이고 과학적인 외상 간호 교육을 통해 실무 역량을 향상시키세요. 
                인터랙티브 시뮬레이션과 실제 임상 사례를 통해 외상 환자 간호의 핵심을 학습할 수 있습니다.
            </p>
        </section>

        <!-- 학습 카드 그리드 -->
        <div class="cards-grid">
            <!-- 외상 기전별 임상 예시 카드 -->
            <a href="#" class="learning-card card-trauma-mechanisms" onclick="openTraumaMechanisms()">
                <span class="card-icon">⚡</span>
                <h2 class="card-title">외상 기전별 임상 예시</h2>
                <p class="card-description">
                    5가지 주요 외상 기전(직접충격, 간접충격, 회전손상, 감속손상, 압축손상)별로 
                    실제 임상 사례와 함께 손상 패턴을 학습하세요.
                </p>
                <ul class="card-features">
                    <li>15개 실제 임상 사례 제공</li>
                    <li>인터랙티브 애니메이션 시뮬레이션</li>
                    <li>손상 기전별 체계적 분류</li>
                    <li>간호 우선순위 및 주의사항</li>
                    <li>심각도별 색상 구분</li>
                </ul>
                <button class="card-button">학습 시작하기 →</button>
            </a>

            <!-- 교통사고 손상 기전 카드 -->
            <a href="#" class="learning-card card-traffic-accidents" onclick="openTrafficAccidents()">
                <span class="card-icon">🚗</span>
                <h2 class="card-title">교통사고 손상 기전</h2>
                <p class="card-description">
                    교통사고 유형별(정면충돌, 측면충돌, 후면충돌, 전복사고) 손상 패턴과 
                    간호사를 위한 체계적 접근법을 학습하세요.
                </p>
                <ul class="card-features">
                    <li>4가지 교통사고 유형별 분석</li>
                    <li>Up & Over vs Down & Under 패턴</li>
                    <li>Whiplash 손상 기전</li>
                    <li>ABCD 체계적 접근법</li>
                    <li>환자 사정 체크리스트</li>
                </ul>
                <button class="card-button">학습 시작하기 →</button>
            </a>
        </div>
    </div>

    <!-- 푸터 -->
    <footer class="footer">
        <div class="footer-content">
            <div class="footer-title">🏥 Trauma Center</div>
            <p>
                학생간호사와 신규간호사의 실무 역량 향상을 위한 전문 교육 플랫폼<br>
                체계적인 외상 간호 교육으로 환자 안전을 지키는 간호사를 양성합니다.
            </p>
            <p style="margin-top: 15px; font-size: 0.9em; opacity: 0.8;">
                © 2024 Trauma Center. 교육 목적으로 제작된 비영리 사이트입니다.
            </p>
        </div>
    </footer>

    <script>
        // 모바일 메뉴 토글
        function toggleMenu() {
            const navMenu = document.getElementById('nav-menu');
            navMenu.classList.toggle('active');
        }

        // 메뉴 바깥쪽 클릭 시 메뉴 닫기
        document.addEventListener('click', function(event) {
            const navMenu = document.getElementById('nav-menu');
            const menuToggle = document.querySelector('.menu-toggle');
            
            if (!navMenu.contains(event.target) && !menuToggle.contains(event.target)) {
                navMenu.classList.remove('active');
            }
        });

        // 외상 기전별 임상 예시 페이지로 이동
        function openTraumaMechanisms() {
            // 새 창에서 외상 기전 페이지 열기
            window.open('trauma-mechanisms.html', '_blank');
        }

        // 교통사고 손상 기전 페이지로 이동  
        function openTrafficAccidents() {
            // 새 창에서 교통사고 페이지 열기
            window.open('traffic-accidents.html', '_blank');
        }

        // 부드러운 스크롤 애니메이션
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                const targetId = this.getAttribute('href').substring(1);
                const targetElement = document.getElementById(targetId);
                
                if (targetElement) {
                    targetElement.scrollIntoView({
                        behavior: 'smooth',
                        block: 'start'
                    });
                }
            });
        });

        // 카드 호버 효과 향상 (터치 디바이스 대응)
        if ('ontouchstart' in window) {
            document.querySelectorAll('.learning-card').forEach(card => {
                card.addEventListener('touchstart', function() {
                    this.style.transform = 'translateY(-5px)';
                });
                
                card.addEventListener('touchend', function() {
                    setTimeout(() => {
                        this.style.transform = 'translateY(0)';
                    }, 150);
                });
            });
        }

        // 페이지 로드 시 애니메이션
        window.addEventListener('load', function() {
            document.body.style.opacity = '1';
        });

        // 스크롤 시 헤더 스타일 변경
        let lastScrollTop = 0;
        window.addEventListener('scroll', function() {
            const header = document.querySelector('.header');
            const scrollTop = window.pageYOffset || document.documentElement.scrollTop;
            
            if (scrollTop > 50) {
                header.style.background = 'rgba(255, 255, 255, 0.98)';
                header.style.boxShadow = '0 2px 25px rgba(0,0,0,0.15)';
            } else {
                header.style.background = 'rgba(255, 255, 255, 0.95)';
                header.style.boxShadow = '0 2px 20px rgba(0,0,0,0.1)';
            }
            
            lastScrollTop = scrollTop;
        });

        // 키보드 네비게이션 지원
        document.addEventListener('keydown', function(e) {
            if (e.key === 'Escape') {
                document.getElementById('nav-menu').classList.remove('active');
            }
        });
    </script>
</body>
</html>
