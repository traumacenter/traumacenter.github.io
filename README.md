<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>낙상 손상 기전 시각화</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #74b9ff 0%, #0984e3 100%);
            color: #333;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
        }
        
        .header {
            text-align: center;
            margin-bottom: 40px;
        }
        
        .header h1 {
            color: #2c3e50;
            font-size: 2.5em;
            margin-bottom: 10px;
        }
        
        .header p {
            color: #7f8c8d;
            font-size: 1.2em;
        }
        
        .controls {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 30px;
        }
        
        .btn {
            padding: 15px 20px;
            border: none;
            border-radius: 10px;
            font-size: 14px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            text-align: center;
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.15);
        }
        
        .btn-low { background: #2ecc71; color: white; }
        .btn-medium { background: #f39c12; color: white; }
        .btn-high { background: #e74c3c; color: white; }
        .btn-feet { background: #3498db; color: white; }
        .btn-head { background: #9b59b6; color: white; }
        .btn-side { background: #1abc9c; color: white; }
        .btn-active { background: #27ae60; color: white; }
        
        .simulation-area {
            display: flex;
            justify-content: center;
            align-items: flex-end;
            min-height: 500px;
            background: linear-gradient(to bottom, #87ceeb 0%, #87ceeb 60%, #90ee90 60%, #90ee90 100%);
            border-radius: 10px;
            margin-bottom: 30px;
            position: relative;
            overflow: hidden;
        }
        
        .building {
            position: absolute;
            left: 100px;
            bottom: 0;
            width: 120px;
            background: #95a5a6;
            border: 2px solid #7f8c8d;
        }
        
        .floor {
            height: 60px;
            border-bottom: 1px solid #7f8c8d;
            position: relative;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #2c3e50;
        }
        
        .person {
            position: absolute;
            transition: all 2s ease-in-out;
        }
        
        .person-body {
            width: 20px;
            height: 40px;
            position: relative;
        }
        
        .head {
            width: 12px;
            height: 12px;
            background: #f4d03f;
            border-radius: 50%;
            margin: 0 auto 2px;
        }
        
        .body {
            width: 16px;
            height: 20px;
            background: #e74c3c;
            margin: 0 auto 2px;
            border-radius: 2px;
        }
        
        .legs {
            width: 16px;
            height: 16px;
            background: #3498db;
            margin: 0 auto;
            border-radius: 2px;
        }
        
        .ground {
            position: absolute;
            bottom: 0;
            width: 100%;
            height: 40px;
            background: #8b4513;
            border-top: 3px solid #654321;
        }
        
        .energy-line {
            position: absolute;
            opacity: 0;
            transition: opacity 0.5s ease;
        }
        
        .injury-indicator {
            position: absolute;
            width: 30px;
            height: 30px;
            background: radial-gradient(circle, rgba(231,76,60,0.8) 0%, rgba(231,76,60,0.2) 100%);
            border-radius: 50%;
            opacity: 0;
            transition: opacity 0.5s ease;
            animation: pulse 1s infinite;
        }
        
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.2); }
        }
        
        @keyframes fall {
            0% { transform: translateY(0) rotate(0deg); }
            100% { transform: translateY(400px) rotate(180deg); }
        }
        
        @keyframes impact {
            0% { transform: scale(1); }
            25% { transform: scale(1.3); }
            50% { transform: scale(0.8); }
            100% { transform: scale(1); }
        }
        
        .fall-animation {
            animation: fall 2s ease-in;
        }
        
        .impact-animation {
            animation: impact 0.5s ease-out;
        }
        
        .stats-panel {
            background: #ecf0f1;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
        }
        
        .stat-item {
            text-align: center;
            padding: 15px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        
        .stat-value {
            font-size: 2em;
            font-weight: bold;
            color: #e74c3c;
            margin-bottom: 5px;
        }
        
        .stat-label {
            font-size: 0.9em;
            color: #7f8c8d;
        }
        
        .explanation {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 10px;
            border-left: 5px solid #3498db;
        }
        
        .explanation h3 {
            color: #2c3e50;
            margin-top: 0;
            font-size: 1.5em;
        }
        
        .injury-list {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }
        
        .injury-item {
            background: white;
            padding: 15px;
            border-radius: 8px;
            border-left: 4px solid #e74c3c;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        
        .injury-item h4 {
            margin: 0 0 10px 0;
            color: #2c3e50;
        }
        
        .severity {
            display: inline-block;
            padding: 3px 8px;
            border-radius: 12px;
            font-size: 0.8em;
            font-weight: bold;
        }
        
        .severity-low { background: #2ecc71; color: white; }
        .severity-medium { background: #f39c12; color: white; }
        .severity-high { background: #e74c3c; color: white; }
        .severity-critical { background: #8e44ad; color: white; }
        
        .hidden {
            display: none;
        }
        
        .trajectory {
            position: absolute;
            stroke: #e74c3c;
            stroke-width: 2;
            stroke-dasharray: 5,5;
            fill: none;
            opacity: 0;
            transition: opacity 0.5s ease;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🏗️ 낙상 손상 기전 시각화</h1>
            <p>높이별, 착지 자세별로 다양한 낙상 상황과 예상 손상을 확인해보세요</p>
        </div>
        
        <div class="controls">
            <button class="btn btn-low" onclick="showFall('low', 'feet')">🟢 저층 발착지<br>(1-2층, 하지 손상)</button>
            <button class="btn btn-medium" onclick="showFall('medium', 'feet')">🟡 중층 발착지<br>(3-4층, 다발 손상)</button>
            <button class="btn btn-high" onclick="showFall('high', 'feet')">🔴 고층 발착지<br>(5층+, 생명 위험)</button>
            <button class="btn btn-head" onclick="showFall('medium', 'head')">🟣 머리 착지<br>(극도 위험)</button>
            <button class="btn btn-side" onclick="showFall('medium', 'side')">🔵 옆으로 착지<br>(측면 손상)</button>
        </div>
        
        <div class="stats-panel" id="stats-panel">
            <div class="stat-item">
                <div class="stat-value" id="height-value">0m</div>
                <div class="stat-label">낙상 높이</div>
            </div>
            <div class="stat-item">
                <div class="stat-value" id="speed-value">0km/h</div>
                <div class="stat-label">충돌 속도</div>
            </div>
            <div class="stat-item">
                <div class="stat-value" id="energy-value">0J</div>
                <div class="stat-label">충격 에너지</div>
            </div>
            <div class="stat-item">
                <div class="stat-value" id="survival-value">100%</div>
                <div class="stat-label">생존율</div>
            </div>
        </div>
        
        <div class="simulation-area" id="simulation-area">
            <div class="building" id="building">
                <div class="floor" id="floor-5">5층</div>
                <div class="floor" id="floor-4">4층</div>
                <div class="floor" id="floor-3">3층</div>
                <div class="floor" id="floor-2">2층</div>
                <div class="floor" id="floor-1">1층</div>
            </div>
            
            <div class="person" id="person">
                <div class="person-body">
                    <div class="head"></div>
                    <div class="body"></div>
                    <div class="legs"></div>
                </div>
            </div>
            
            <div class="ground"></div>
            
            <!-- 손상 지시자들 -->
            <div class="injury-indicator" id="head-injury" style="left: 300px; bottom: 30px;"></div>
            <div class="injury-indicator" id="spine-injury" style="left: 300px; bottom: 50px;"></div>
            <div class="injury-indicator" id="leg-injury" style="left: 300px; bottom: 10px;"></div>
            <div class="injury-indicator" id="organ-injury" style="left: 300px; bottom: 35px;"></div>
            
            <!-- 에너지 전달 라인 -->
            <svg class="trajectory" id="energy-path" width="400" height="500">
                <path d="M 300 450 L 300 400 L 300 350 L 300 300 L 300 250" stroke="#e74c3c" stroke-width="3"/>
            </svg>
        </div>
        
        <div class="explanation" id="explanation">
            <h3>🔍 낙상 시나리오를 선택해주세요</h3>
            <p>위의 버튼을 클릭하여 다양한 낙상 상황별 손상 기전과 예상 손상 부위를 확인해보세요.</p>
        </div>
    </div>

    <script>
        const fallScenarios = {
            'low-feet': {
                title: "🟢 저층 낙상 - 발부터 착지 (1-2층)",
                height: 6,
                speed: 35,
                energy: 4116,
                survival: 95,
                startFloor: 2,
                landingType: 'feet',
                content: `
                    <p><strong>🏗️ 시나리오:</strong> 건설현장에서 2층 높이 사다리(6m)에서 발부터 떨어짐</p>
                    <div class="injury-list">
                        <div class="injury-item">
                            <h4>🦶 1차 충격 부위</h4>
                            <span class="severity severity-medium">중등도</span>
                            <p>• 종골(발뒤꿈치) 골절<br>
                            • 발목 관절 손상<br>
                            • 족부 인대 파열</p>
                        </div>
                        <div class="injury-item">
                            <h4>🦵 에너지 전달 부위</h4>
                            <span class="severity severity-low">경미</span>
                            <p>• 경골/비골 골절 가능성<br>
                            • 무릎 반월판 손상<br>
                            • 대퇴부 타박상</p>
                        </div>
                        <div class="injury-item">
                            <h4>🏥 간호 중점사항</h4>
                            <span class="severity severity-low">관찰</span>
                            <p>• 하지 혈관 상태 확인<br>
                            • 발가락 색깔, 온도 체크<br>
                            • 통증 관리 및 고정</p>
                        </div>
                    </div>
                    <p><strong>💡 예후:</strong> 적절한 치료 시 완전 회복 가능, 보행 기능 정상 회복 예상</p>
                `
            },
            'medium-feet': {
                title: "🟡 중층 낙상 - 발부터 착지 (3-4층)",
                height: 12,
                speed: 49,
                energy: 8232,
                survival: 60,
                startFloor: 4,
                landingType: 'feet',
                content: `
                    <p><strong>🏗️ 시나리오:</strong> 4층 아파트(12m)에서 음주 후 발부터 추락한 30세 남성</p>
                    <div class="injury-list">
                        <div class="injury-item">
                            <h4>🦶 하지 손상 (Don Juan Syndrome)</h4>
                            <span class="severity severity-high">심각</span>
                            <p>• 양측 종골 분쇄골절<br>
                            • 경골/비골 개방성 골절<br>
                            • 무릎/고관절 손상</p>
                        </div>
                        <div class="injury-item">
                            <h4>🦴 척추 손상</h4>
                            <span class="severity severity-high">심각</span>
                            <p>• L1-L2 압박골절<br>
                            • 척수 손상 가능성<br>
                            • 하반신 기능 장애</p>
                        </div>
                        <div class="injury-item">
                            <h4>🫀 내장기관 손상</h4>
                            <span class="severity severity-medium">중등도</span>
                            <p>• 간 열상 (우상복부 통증)<br>
                            • 비장 타박 (좌상복부)<br>
                            • 내출혈 가능성</p>
                        </div>
                        <div class="injury-item">
                            <h4>🚨 응급 처치</h4>
                            <span class="severity severity-critical">최우선</span>
                            <p>• 척추 고정 및 경추 보호<br>
                            • 내출혈 모니터링<br>
                            • 다발성 외상 프로토콜</p>
                        </div>
                    </div>
                    <p><strong>⚠️ 주의:</strong> 생명 위험 구간 진입, 즉시 외상센터 이송 및 다학제 치료 필요</p>
                `
            },
            'high-feet': {
                title: "🔴 고층 낙상 - 발부터 착지 (5층 이상)",
                height: 18,
                speed: 60,
                energy: 12348,
                survival: 20,
                startFloor: 5,
                landingType: 'feet',
                content: `
                    <p><strong>🏗️ 시나리오:</strong> 6층 건물 옥상(18m)에서 자살 시도로 발부터 낙상한 28세 청년</p>
                    <div class="injury-list">
                        <div class="injury-item">
                            <h4>🦶 하지 완전 파괴</h4>
                            <span class="severity severity-critical">치명적</span>
                            <p>• 양측 종골 완전 분쇄<br>
                            • 다발성 개방성 골절<br>
                            • 혈관, 신경 손상</p>
                        </div>
                        <div class="injury-item">
                            <h4>🦴 척추 폭발성 골절</h4>
                            <span class="severity severity-critical">치명적</span>
                            <p>• L1, L2 폭발성 골절<br>
                            • 척수 완전 손상<br>
                            • 하반신 완전 마비</p>
                        </div>
                        <div class="injury-item">
                            <h4>🫀 다발성 장기 손상</h4>
                            <span class="severity severity-critical">치명적</span>
                            <p>• 간 다발성 열상<br>
                            • 비장 파열<br>
                            • 신장 손상<br>
                            • 대량 내출혈</p>
                        </div>
                        <div class="injury-item">
                            <h4>🚨 소생술</h4>
                            <span class="severity severity-critical">즉시</span>
                            <p>• 출혈성 쇼크 치료<br>
                            • 응급 수술 준비<br>
                            • 대량 수혈 프로토콜<br>
                            • 가족 상담</p>
                        </div>
                    </div>
                    <p><strong>💀 예후:</strong> 극도로 불량, 생존 시에도 영구 장애 불가피</p>
                `
            },
            'medium-head': {
                title: "🟣 머리부터 착지 - 극도 위험",
                height: 12,
                speed: 49,
                energy: 8232,
                survival: 5,
                startFloor: 4,
                landingType: 'head',
                content: `
                    <p><strong>🏗️시나리오:</strong> 4층 건물 옥상(12m)에서 머리부터 떨어진 22세 대학생</p>
                    <div class="injury-list">
                        <div class="injury-item">
                            <h4>🧠 뇌 손상</h4>
                            <span class="severity severity-critical">치명적</span>
                            <p>• 두개골 기저부 골절<br>
                            • 경막하/경막외 혈종<br>
                            • 뇌좌상, 뇌부종<br>
                            • 뇌척수액 누출</p>
                        </div>
                        <div class="injury-item">
                            <h4>🦴 경추 손상</h4>
                            <span class="severity severity-critical">치명적</span>
                            <p>• C1-C2 골절/탈구<br>
                            • 경추 불안정성<br>
                            • 척수 완전 손상<br>
                            • 호흡근 마비</p>
                        </div>
                        <div class="injury-item">
                            <h4>🫁 흉부 손상</h4>
                            <span class="severity severity-critical">치명적</span>
                            <p>• 대동맥 파열<br>
                            • 양측 기흉<br>
                            • 폐 좌상<br>
                            • 다발성 늑골 골절</p>
                        </div>
                        <div class="injury-item">
                            <h4>🚨 응급 처치</h4>
                            <span class="severity severity-critical">즉시</span>
                            <p>• 기도 확보 (경추 보호)<br>
                            • 인공호흡 시작<br>
                            • 뇌압 조절<br>
                            • 응급 신경외과 수술</p>
                        </div>
                    </div>
                    <p><strong>☠️ 예후:</strong> 거의 사망, 생존 시 식물인간 상태 또는 심각한 장애</p>
                `
            },
            'medium-side': {
                title: "🔵 옆으로 착지 - 측면 손상",
                height: 9,
                speed: 43,
                energy: 6174,
                survival: 75,
                startFloor: 3,
                landingType: 'side',
                content: `
                    <p><strong>🏗️ 시나리오:</strong> 3층 베란다(9m)에서 빨래를 널다가 왼쪽으로 떨어진 45세 여성</p>
                    <div class="injury-list">
                        <div class="injury-item">
                            <h4>💪 상지 손상</h4>
                            <span class="severity severity-medium">중등도</span>
                            <p>• 왼팔 요골 골절<br>
                            • 어깨 탈구<br>
                            • 쇄골 골절<br>
                            • 손목 골절</p>
                        </div>
                        <div class="injury-item">
                            <h4>🫁 흉부 손상</h4>
                            <span class="severity severity-medium">중등도</span>
                            <p>• 좌측 늑골 골절(5-7번)<br>
                            • 기흉 가능성<br>
                            • 폐 좌상<br>
                            • 흉벽 타박상</p>
                        </div>
                        <div class="injury-item">
                            <h4>🦴 골반 손상</h4>
                            <span class="severity severity-medium">중등도</span>
                            <p>• 좌측 장골 골절<br>
                            • 고관절 타박<br>
                            • 골반 인대 손상</p>
                        </div>
                        <div class="injury-item">
                            <h4>🫀 내장기관</h4>
                            <span class="severity severity-low">경미</span>
                            <p>• 비장 타박 (좌상복부)<br>
                            • 신장 타박<br>
                            • 내출혈 모니터링 필요</p>
                        </div>
                    </div>
                    <p><strong>💡 특징:</strong> 한쪽 집중 손상, 반대편은 상대적으로 안전</p>
                `
            }
        };

        function showFall(height, landingType) {
            const scenarioKey = `${height}-${landingType}`;
            const scenario = fallScenarios[scenarioKey];
            
            if (!scenario) return;
            
            // 모든 버튼 비활성화
            document.querySelectorAll('.btn').forEach(btn => {
                btn.classList.remove('btn-active');
            });
            
            // 클릭된 버튼 활성화
            event.target.classList.add('btn-active');
            
            // 통계 업데이트
            updateStats(scenario);
            
            // 애니메이션 시작
            animateFall(scenario);
            
            // 설명 업데이트
            updateExplanation(scenario);
        }

        function updateStats(scenario) {
            document.getElementById('height-value').textContent = scenario.height + 'm';
            document.getElementById('speed-value').textContent = scenario.speed + 'km/h';
            document.getElementById('energy-value').textContent = scenario.energy + 'J';
            document.getElementById('survival-value').textContent = scenario.survival + '%';
            
            // 생존율에 따른 색상 변경
            const survivalElement = document.getElementById('survival-value');
            if (scenario.survival >= 80) {
                survivalElement.style.color = '#2ecc71';
            } else if (scenario.survival >= 50) {
                survivalElement.style.color = '#f39c12';
            } else {
                survivalElement.style.color = '#e74c3c';
            }
        }

        function animateFall(scenario) {
            const person = document.getElementById('person');
            const building = document.getElementById('building');
            
            // 초기 위치 설정
            const startHeight = (scenario.startFloor - 1) * 60 + 30;
            person.style.left = '130px';
            person.style.bottom = startHeight + 'px';
            person.style.transform = 'rotate(0deg)';
            
            // 건물 높이 조정
            const floors = building.querySelectorAll('.floor');
            floors.forEach((floor, index) => {
                if (index < scenario.startFloor) {
                    floor.style.display = 'block';
                } else {
                    floor.style.display = 'none';
                }
            });
            
            // 손상 지시자 숨기기
            document.querySelectorAll('.injury-indicator').forEach(indicator => {
                indicator.style.opacity = '0';
            });
            
            // 낙하 애니메이션
            setTimeout(() => {
                person.style.left = '280px';
                person.style.bottom = '40px';
                
                // 착지 자세에 따른 회전
                if (scenario.landingType === 'head') {
                    person.style.transform = 'rotate(180deg)';
                } else if (scenario.landingType === 'side') {
                    person.style.transform = 'rotate(90deg)';
                } else {
                    person.style.transform = 'rotate(0deg)';
                }
                
                // 충격 후 손상 지시자 표시
                setTimeout(() => {
                    showInjuries(scenario);
                }, 1500);
                
            }, 500);
        }

        function showInjuries(scenario) {
            const injuryMapping = {
                'low-feet': ['leg-injury'],
                'medium-feet': ['leg-injury', 'spine-injury', 'organ-injury'],
                'high-feet': ['leg-injury', 'spine-injury', 'organ-injury', 'head-injury'],
                'medium-head': ['head-injury', 'spine-injury', 'organ-injury'],
                'medium-side': ['leg-injury', 'organ-injury']
            };
            
            const scenarioKey = `${scenario.startFloor <= 2 ? 'low' : scenario.startFloor <= 4 ? 'medium' : 'high'}-${scenario.landingType}`;
            const injuries = injuryMapping[scenarioKey] || [];
            
            injuries.forEach((injuryId, index) => {
                setTimeout(() => {
                    const indicator = document.getElementById(injuryId);
                    if (indicator) {
                        indicator.style.opacity = '1';
                    }
                }, index * 300);
            });
            
            // 에너지 전달 경로 표시
            setTimeout(() => {
                const energyPath = document.getElementById('energy-path');
                energyPath.style.opacity = '0.7';
                
                setTimeout(() => {
                    energyPath.style.opacity = '0';
                }, 2000);
            }, 1000);
        }

        function updateExplanation(scenario) {
            const explanation = document.getElementById('explanation');
            explanation.innerHTML = `
                <h3>${scenario.title}</h3>
                ${scenario.content}
            `;
        }

        // 페이지 로드 시 기본 시나리오 표시
        window.addEventListener('load', () => {
            showFall('low', 'feet');
        });
    </script>
</body>
</html>
