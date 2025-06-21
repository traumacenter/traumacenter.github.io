<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>교통사고 손상 기전 시각화</title>
    <style>
        * {
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 10px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
            min-height: 100vh;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            width: 100%;
        }
        
        .header {
            text-align: center;
            margin-bottom: 30px;
        }
        
        .header h1 {
            color: #2c3e50;
            font-size: clamp(1.8em, 4vw, 2.5em);
            margin-bottom: 10px;
            line-height: 1.2;
        }
        
        .header p {
            color: #7f8c8d;
            font-size: clamp(1em, 2.5vw, 1.2em);
            line-height: 1.4;
            margin: 0 10px;
        }
        
        .controls {
            display: flex;
            justify-content: center;
            gap: 12px;
            margin-bottom: 25px;
            flex-wrap: wrap;
        }
        
        .btn {
            padding: 12px 20px;
            border: none;
            border-radius: 25px;
            font-size: clamp(12px, 3vw, 16px);
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            text-align: center;
            line-height: 1.3;
            min-width: 120px;
        }
        
        /* 모바일에서 버튼 조정 */
        @media (max-width: 768px) {
            .controls {
                display: grid;
                grid-template-columns: repeat(2, 1fr);
                gap: 10px;
            }
            
            .btn {
                min-width: auto;
                padding: 15px 10px;
                font-size: 13px;
            }
        }
        
        @media (max-width: 480px) {
            body {
                padding: 5px;
            }
            
            .container {
                padding: 15px;
                border-radius: 10px;
            }
            
            .controls {
                grid-template-columns: 1fr;
            }
            
            .btn {
                padding: 12px 8px;
                font-size: 12px;
            }
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.15);
        }
        
        /* 터치 디바이스에서 호버 효과 비활성화 */
        @media (hover: none) {
            .btn:hover {
                transform: none;
                box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            }
        }
        
        .btn-front { background: #e74c3c; color: white; }
        .btn-side { background: #f39c12; color: white; }
        .btn-rear { background: #3498db; color: white; }
        .btn-rollover { background: #9b59b6; color: white; }
        .btn-active { background: #27ae60; color: white; }
        
        .simulation-area {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 300px;
            height: 50vh;
            max-height: 400px;
            background: #f8f9fa;
            border-radius: 10px;
            margin-bottom: 25px;
            position: relative;
            overflow: hidden;
            width: 100%;
        }
        
        /* 모바일에서 시뮬레이션 영역 조정 */
        @media (max-width: 768px) {
            .simulation-area {
                min-height: 250px;
                height: 40vh;
                margin-bottom: 20px;
            }
        }
        
        @media (max-width: 480px) {
            .simulation-area {
                min-height: 200px;
                height: 35vh;
            }
        }
        
        .road {
            position: absolute;
            bottom: 0;
            width: 100%;
            height: clamp(40px, 8vh, 60px);
            background: #34495e;
            background-image: repeating-linear-gradient(
                90deg,
                transparent,
                transparent 10px,
                #fff 10px,
                #fff 30px
            );
        }
        
        svg {
            max-width: 800px;
            width: 100%;
            height: 100%;
            max-height: 400px;
        }
        
        .car {
            transition: all 0.8s ease;
        }
        
        .person {
            transition: all 0.8s ease;
        }
        
        .impact-line {
            stroke: #e74c3c;
            stroke-width: 3;
            stroke-dasharray: 5,5;
            opacity: 0;
            transition: opacity 0.5s ease;
        }
        
        .injury-zone {
            fill: rgba(231, 76, 60, 0.3);
            opacity: 0;
            transition: opacity 0.5s ease;
        }
        
        .explanation {
            background: #ecf0f1;
            padding: clamp(15px, 4vw, 25px);
            border-radius: 10px;
            border-left: 5px solid #3498db;
        }
        
        .explanation h3 {
            color: #2c3e50;
            margin-top: 0;
            font-size: clamp(1.2em, 3.5vw, 1.5em);
            line-height: 1.3;
        }
        
        .explanation p {
            font-size: clamp(0.9em, 2.5vw, 1em);
            line-height: 1.5;
            margin: 10px 0;
        }
        
        .injury-list {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }
        
        /* 모바일에서 손상 리스트 조정 */
        @media (max-width: 768px) {
            .injury-list {
                grid-template-columns: 1fr;
                gap: 12px;
                margin-top: 15px;
            }
        }
        
        .injury-item {
            background: white;
            padding: clamp(12px, 3vw, 15px);
            border-radius: 8px;
            border-left: 4px solid #e74c3c;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        
        .injury-item h4 {
            margin: 0 0 8px 0;
            color: #2c3e50;
            font-size: clamp(0.9em, 2.5vw, 1em);
            line-height: 1.3;
        }
        
        .injury-item p {
            font-size: clamp(0.8em, 2.2vw, 0.9em);
            line-height: 1.4;
            margin: 5px 0;
        }
        
        .hidden {
            display: none;
        }
        
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            75% { transform: translateX(5px); }
        }
        
        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }
        
        .animate-shake {
            animation: shake 0.6s ease-in-out;
        }
        
        .animate-bounce {
            animation: bounce 0.8s ease-in-out;
        }
        
        /* 가로 모드 최적화 */
        @media (max-width: 768px) and (orientation: landscape) {
            .simulation-area {
                height: 60vh;
                min-height: 250px;
            }
        }
        
        /* 아주 작은 화면 최적화 */
        @media (max-width: 320px) {
            .header h1 {
                font-size: 1.5em;
            }
            
            .btn {
                font-size: 11px;
                padding: 10px 6px;
            }
            
            .simulation-area {
                min-height: 180px;
                height: 30vh;
            }
            
            .explanation {
                padding: 12px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🚗 교통사고 손상 기전 시각화</h1>
            <p>각 버튼을 클릭하여 다양한 교통사고 유형과 손상 기전을 확인해보세요</p>
        </div>
        
        <div class="controls">
            <button class="btn btn-front" onclick="showCollision('front')">정면 충돌</button>
            <button class="btn btn-side" onclick="showCollision('side')">측면 충돌</button>
            <button class="btn btn-rear" onclick="showCollision('rear')">후면 충돌</button>
            <button class="btn btn-rollover" onclick="showCollision('rollover')">전복 사고</button>
        </div>
        
        <div class="simulation-area">
            <div class="road"></div>
            <svg viewBox="0 0 800 400" id="collision-svg">
                <!-- 정면 충돌 -->
                <g id="front-collision" class="hidden">
                    <!-- 벽/장애물 -->
                    <rect x="620" y="150" width="30" height="100" fill="#95a5a6" stroke="#7f8c8d" stroke-width="2"/>
                    
                    <!-- 자동차 -->
                    <g class="car" id="front-car">
                        <rect x="450" y="180" width="120" height="60" rx="10" fill="#3498db" stroke="#2980b9" stroke-width="2"/>
                        <circle cx="470" cy="250" r="15" fill="#2c3e50"/>
                        <circle cx="550" cy="250" r="15" fill="#2c3e50"/>
                        <rect x="460" y="190" width="100" height="40" rx="5" fill="#5dade2"/>
                        
                        <!-- 승객 (Up and Over) -->
                        <g class="person" id="person-up">
                            <circle cx="500" cy="200" r="8" fill="#f4d03f"/>
                            <rect x="496" y="208" width="8" height="15" fill="#e74c3c"/>
                            <line x1="496" y1="215" x2="490" y2="230" stroke="#2c3e50" stroke-width="2"/>
                            <line x1="504" y1="215" x2="510" y2="230" stroke="#2c3e50" stroke-width="2"/>
                        </g>
                        
                        <!-- 승객 (Down and Under) -->
                        <g class="person" id="person-down">
                            <circle cx="520" cy="205" r="8" fill="#f4d03f"/>
                            <rect x="516" y="213" width="8" height="15" fill="#e74c3c"/>
                            <line x1="516" y1="220" x2="510" y2="235" stroke="#2c3e50" stroke-width="2"/>
                            <line x1="524" y1="220" x2="530" y2="235" stroke="#2c3e50" stroke-width="2"/>
                        </g>
                    </g>
                    
                    <!-- 충격 방향 화살표 -->
                    <path d="M 350 210 L 430 210" stroke="#e74c3c" stroke-width="4" fill="none" marker-end="url(#arrowhead)"/>
                    
                    <!-- 손상 구역 -->
                    <ellipse cx="500" cy="180" rx="40" ry="20" class="injury-zone" id="head-injury"/>
                    <ellipse cx="520" cy="240" rx="35" ry="15" class="injury-zone" id="knee-injury"/>
                </g>
                
                <!-- 측면 충돌 -->
                <g id="side-collision" class="hidden">
                    <!-- 충돌하는 차 -->
                    <g class="car" id="side-car-1">
                        <rect x="200" y="100" width="60" height="120" rx="10" fill="#e74c3c" stroke="#c0392b" stroke-width="2"/>
                        <circle cx="180" cy="120" r="15" fill="#2c3e50"/>
                        <circle cx="180" cy="200" r="15" fill="#2c3e50"/>
                        <rect x="210" y="110" width="40" height="100" rx="5" fill="#ec7063"/>
                    </g>
                    
                    <!-- 피해 차량 -->
                    <g class="car" id="side-car-2">
                        <rect x="350" y="180" width="120" height="60" rx="10" fill="#3498db" stroke="#2980b9" stroke-width="2"/>
                        <circle cx="370" cy="250" r="15" fill="#2c3e50"/>
                        <circle cx="450" cy="250" r="15" fill="#2c3e50"/>
                        <rect x="360" y="190" width="100" height="40" rx="5" fill="#5dade2"/>
                        
                        <!-- 승객 -->
                        <g class="person" id="side-person">
                            <circle cx="400" cy="205" r="8" fill="#f4d03f"/>
                            <rect x="396" y="213" width="8" height="15" fill="#e74c3c"/>
                            <line x1="396" y1="220" x2="390" y2="235" stroke="#2c3e50" stroke-width="2"/>
                            <line x1="404" y1="220" x2="410" y2="235" stroke="#2c3e50" stroke-width="2"/>
                        </g>
                    </g>
                    
                    <!-- 충격 방향 화살표 -->
                    <path d="M 230 160 L 340 200" stroke="#e74c3c" stroke-width="4" fill="none" marker-end="url(#arrowhead)"/>
                    
                    <!-- 손상 구역 -->
                    <ellipse cx="370" cy="205" rx="25" ry="40" class="injury-zone" id="side-injury"/>
                </g>
                
                <!-- 후면 충돌 -->
                <g id="rear-collision" class="hidden">
                    <!-- 후방 차량 -->
                    <g class="car" id="rear-car-1">
                        <rect x="200" y="180" width="120" height="60" rx="10" fill="#e74c3c" stroke="#c0392b" stroke-width="2"/>
                        <circle cx="220" cy="250" r="15" fill="#2c3e50"/>
                        <circle cx="300" cy="250" r="15" fill="#2c3e50"/>
                        <rect x="210" y="190" width="100" height="40" rx="5" fill="#ec7063"/>
                    </g>
                    
                    <!-- 앞 차량 -->
                    <g class="car" id="rear-car-2">
                        <rect x="450" y="180" width="120" height="60" rx="10" fill="#3498db" stroke="#2980b9" stroke-width="2"/>
                        <circle cx="470" cy="250" r="15" fill="#2c3e50"/>
                        <circle cx="550" cy="250" r="15" fill="#2c3e50"/>
                        <rect x="460" y="190" width="100" height="40" rx="5" fill="#5dade2"/>
                        
                        <!-- 승객 (Whiplash) -->
                        <g class="person" id="whiplash-person">
                            <circle cx="500" cy="205" r="8" fill="#f4d03f"/>
                            <rect x="496" y="213" width="8" height="15" fill="#e74c3c"/>
                            <line x1="496" y1="220" x2="490" y2="235" stroke="#2c3e50" stroke-width="2"/>
                            <line x1="504" y1="220" x2="510" y2="235" stroke="#2c3e50" stroke-width="2"/>
                            <!-- 목 움직임 표시 -->
                            <path d="M 500 205 Q 485 190 475 185" stroke="#e74c3c" stroke-width="2" fill="none" stroke-dasharray="3,3"/>
                            <path d="M 500 205 Q 515 185 525 180" stroke="#f39c12" stroke-width="2" fill="none" stroke-dasharray="3,3"/>
                        </g>
                    </g>
                    
                    <!-- 충격 방향 화살표 -->
                    <path d="M 340 210 L 430 210" stroke="#e74c3c" stroke-width="4" fill="none" marker-end="url(#arrowhead)"/>
                    
                    <!-- 손상 구역 -->
                    <ellipse cx="500" cy="190" rx="30" ry="15" class="injury-zone" id="neck-injury"/>
                </g>
                
                <!-- 전복 사고 -->
                <g id="rollover-collision" class="hidden">
                    <!-- 뒤집힌 차량 -->
                    <g class="car" id="rollover-car" transform="rotate(45 400 200)">
                        <rect x="350" y="180" width="120" height="60" rx="10" fill="#9b59b6" stroke="#8e44ad" stroke-width="2"/>
                        <circle cx="370" cy="250" r="15" fill="#2c3e50"/>
                        <circle cx="450" cy="250" r="15" fill="#2c3e50"/>
                        <rect x="360" y="190" width="100" height="40" rx="5" fill="#bb8fce"/>
                        
                        <!-- 승객 -->
                        <g class="person" id="rollover-person">
                            <circle cx="400" cy="205" r="8" fill="#f4d03f"/>
                            <rect x="396" y="213" width="8" height="15" fill="#e74c3c"/>
                            <line x1="396" y1="220" x2="390" y2="235" stroke="#2c3e50" stroke-width="2"/>
                            <line x1="404" y1="220" x2="410" y2="235" stroke="#2c3e50" stroke-width="2"/>
                        </g>
                    </g>
                    
                    <!-- 회전 화살표 -->
                    <path d="M 400 120 A 80 80 0 1 1 480 200" stroke="#e74c3c" stroke-width="3" fill="none" marker-end="url(#arrowhead)" stroke-dasharray="5,5"/>
                    
                    <!-- 다발성 손상 구역 -->
                    <ellipse cx="400" cy="200" rx="60" ry="40" class="injury-zone" id="multiple-injury"/>
                </g>
                
                <!-- 화살표 마커 정의 -->
                <defs>
                    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="10" refY="3.5" orient="auto">
                        <polygon points="0 0, 10 3.5, 0 7" fill="#e74c3c"/>
                    </marker>
                </defs>
            </svg>
        </div>
        
        <div class="explanation" id="explanation">
            <h3>🔍 교통사고 유형을 선택해주세요</h3>
            <p>위의 버튼을 클릭하여 각 교통사고 유형별 손상 기전과 예상 손상 부위를 확인해보세요.</p>
        </div>
    </div>

    <script>
        const explanations = {
            front: {
                title: "🚗 정면 충돌 (Frontal Collision)",
                content: `
                    <p><strong>발생 기전:</strong> 차량이 정면으로 충돌하면서 승객이 관성에 의해 앞으로 밀려나는 상황</p>
                    <div class="injury-list">
                        <div class="injury-item">
                            <h4>🔴 Up and Over 패턴</h4>
                            <p>• 머리/목: 두개골 골절, 경추 손상, 뇌출혈<br>
                            • 흉부: 늑골 골절, 기흉, 심장 좌상<br>
                            • 복부: 간/비장 파열, 내출혈</p>
                        </div>
                        <div class="injury-item">
                            <h4>🟡 Down and Under 패턴</h4>
                            <p>• 무릎: 슬개골 골절, 인대 파열<br>
                            • 대퇴부: 대퇴골 간부 골절<br>
                            • 고관절: 후방 탈구, 골반 골절</p>
                        </div>
                    </div>
                    <p><strong>💡 핵심:</strong> 안전벨트 착용 여부에 따라 손상 패턴이 달라지며, 속도가 높을수록 더 심각한 손상이 발생합니다.</p>
                `
            },
            side: {
                title: "↔️ 측면 충돌 (Side Impact)",
                content: `
                    <p><strong>발생 기전:</strong> 차량 측면에 충격이 가해지면서 승객이 충돌 방향으로 밀려나는 상황</p>
                    <div class="injury-list">
                        <div class="injury-item">
                            <h4>🔴 직접 충격 손상</h4>
                            <p>• 머리: 측두골 골절, 뇌출혈<br>
                            • 흉부: 늑골 골절, 기흉<br>
                            • 골반: 골반골 골절, 고관절 탈구</p>
                        </div>
                        <div class="injury-item">
                            <h4>🟡 내장기관 손상</h4>
                            <p>• 간/비장: 늑골 압박으로 인한 파열<br>
                            • 신장: 측면 압박 손상<br>
                            • 폐: 기흉, 혈흉</p>
                        </div>
                    </div>
                    <p><strong>⚠️ 주의:</strong> 측면은 보호 구조가 약해 고속 충돌 시 매우 위험하며, 충돌 방향 쪽 집중적 손상이 특징입니다.</p>
                `
            },
            rear: {
                title: "⬅️ 후면 충돌 (Rear Impact)",
                content: `
                    <p><strong>발생 기전:</strong> 후방에서 충돌하면서 승객의 머리가 채찍처럼 뒤로 젖혀졌다가 앞으로 꺾이는 상황</p>
                    <div class="injury-list">
                        <div class="injury-item">
                            <h4>🔴 편타성 손상 (Whiplash)</h4>
                            <p>• 1단계: 목 과신전 (뒤로 젖혀짐)<br>
                            • 2단계: 목 과굴곡 (앞으로 꺾임)<br>
                            • 결과: 경추 염좌, 근육/인대 손상</p>
                        </div>
                        <div class="injury-item">
                            <h4>🟡 기타 손상</h4>
                            <p>• 요추: 좌석등받이 압박 손상<br>
                            • 흉부: 안전벨트에 의한 늑골 골절<br>
                            • 심리: PTSD, 운전 공포증</p>
                        </div>
                    </div>
                    <p><strong>💡 특징:</strong> 즉시 증상이 나타나지 않고 24-48시간 후 증상이 악화되는 경우가 많습니다.</p>
                `
            },
            rollover: {
                title: "🔄 전복 사고 (Rollover)",
                content: `
                    <p><strong>발생 기전:</strong> 차량이 뒤집히거나 굴러가면서 승객이 차 안에서 여러 방향으로 충격을 받는 상황</p>
                    <div class="injury-list">
                        <div class="injury-item">
                            <h4>🔴 다발성 손상</h4>
                            <p>• 머리: 반복적 충격으로 심각한 뇌외상<br>
                            • 척추: 다방향 힘으로 복잡한 골절<br>
                            • 사지: 여러 부위 골절과 탈구</p>
                        </div>
                        <div class="injury-item">
                            <h4>🟡 특수 위험</h4>
                            <p>• 차 밖 이탈: 압사 위험<br>
                            • 지붕 압궤: 경추 압박골절<br>
                            • 화재: 화상 위험</p>
                        </div>
                    </div>
                    <p><strong>⚠️ 최고 위험:</strong> 가장 예측하기 어렵고 다발성 손상 위험이 높아 체계적이고 신속한 전신 검사가 필요합니다.</p>
                `
            }
        };

        function showCollision(type) {
            // 모든 시뮬레이션 숨기기
            document.querySelectorAll('#collision-svg > g').forEach(g => {
                g.classList.add('hidden');
            });
            
            // 모든 버튼 비활성화
            document.querySelectorAll('.btn').forEach(btn => {
                btn.classList.remove('btn-active');
            });
            
            // 선택된 시뮬레이션 보이기
            const selectedGroup = document.getElementById(type + '-collision');
            if (selectedGroup) {
                selectedGroup.classList.remove('hidden');
                
                // 애니메이션 효과
                setTimeout(() => {
                    const cars = selectedGroup.querySelectorAll('.car');
                    const persons = selectedGroup.querySelectorAll('.person');
                    const injuries = selectedGroup.querySelectorAll('.injury-zone');
                    
                    cars.forEach(car => car.classList.add('animate-shake'));
                    persons.forEach(person => person.classList.add('animate-bounce'));
                    
                    setTimeout(() => {
                        injuries.forEach(injury => {
                            injury.style.opacity = '1';
                        });
                    }, 800);
                    
                    setTimeout(() => {
                        cars.forEach(car => car.classList.remove('animate-shake'));
                        persons.forEach(person => person.classList.remove('animate-bounce'));
                    }, 1000);
                }, 100);
            }
            
            // 버튼 활성화
            event.target.classList.add('btn-active');
            
            // 설명 업데이트
            const explanation = document.getElementById('explanation');
            if (explanations[type]) {
                explanation.innerHTML = `
                    <h3>${explanations[type].title}</h3>
                    ${explanations[type].content}
                `;
            }
        }

        // 페이지 로드 시 정면 충돌을 기본으로 표시
        window.addEventListener('load', () => {
            showCollision('front');
            document.querySelector('.btn-front').classList.add('btn-active');
        });
    </script>
</body>
</html>
