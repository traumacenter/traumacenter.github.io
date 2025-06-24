<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Down and Under 패턴 교육 슬라이드</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Malgun Gothic', 'Arial', sans-serif;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            color: #2c3e50;
            line-height: 1.6;
        }

        .presentation-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
            padding: 30px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
        }

        .header h1 {
            font-size: 2.5em;
            color: #e74c3c;
            margin-bottom: 10px;
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .header .subtitle {
            font-size: 1.2em;
            color: #7f8c8d;
            margin-bottom: 15px;
        }

        .header .description {
            font-size: 1em;
            color: #34495e;
            max-width: 800px;
            margin: 0 auto;
        }

        .slide-navigation {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }

        .slide-nav-btn {
            padding: 12px 20px;
            background: white;
            border: 2px solid #3498db;
            color: #3498db;
            border-radius: 25px;
            cursor: pointer;
            font-size: 14px;
            font-weight: bold;
            transition: all 0.3s ease;
            text-align: center;
            min-width: 120px;
        }

        .slide-nav-btn:hover {
            background: #3498db;
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(52,152,219,0.3);
        }

        .slide-nav-btn.active {
            background: #e74c3c;
            border-color: #e74c3c;
            color: white;
            box-shadow: 0 5px 20px rgba(231,76,60,0.4);
        }

        .slide {
            display: none;
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 40px rgba(0,0,0,0.1);
            overflow: hidden;
            margin-bottom: 20px;
            min-height: 700px;
        }

        .slide.active {
            display: block;
            animation: slideIn 0.5s ease-out;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .slide-header {
            background: linear-gradient(135deg, #e74c3c, #c0392b);
            color: white;
            padding: 25px;
            text-align: center;
        }

        .slide-title {
            font-size: 2em;
            font-weight: bold;
            margin-bottom: 10px;
            text-shadow: 0 2px 4px rgba(0,0,0,0.3);
        }

        .slide-subtitle {
            font-size: 1.1em;
            opacity: 0.9;
        }

        .slide-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            padding: 30px;
            align-items: center;
            min-height: 500px;
        }

        .slide-visual {
            display: flex;
            justify-content: center;
            align-items: center;
            background: #f8f9fa;
            border-radius: 15px;
            padding: 20px;
            box-shadow: inset 0 2px 10px rgba(0,0,0,0.05);
        }

        .slide-description {
            padding: 20px;
        }

        .slide-description h3 {
            color: #2c3e50;
            font-size: 1.3em;
            margin-bottom: 20px;
            border-bottom: 3px solid #3498db;
            padding-bottom: 10px;
        }

        .slide-description ul {
            list-style: none;
            padding: 0;
        }

        .slide-description li {
            margin-bottom: 15px;
            padding-left: 25px;
            position: relative;
            font-size: 1.1em;
            line-height: 1.7;
        }

        .slide-description li::before {
            content: "▸";
            position: absolute;
            left: 0;
            color: #e74c3c;
            font-weight: bold;
            font-size: 1.2em;
        }

        .key-points {
            background: #ecf0f1;
            border-left: 5px solid #e74c3c;
            padding: 20px;
            border-radius: 8px;
            margin-top: 20px;
        }

        .key-points h4 {
            color: #e74c3c;
            margin-bottom: 10px;
            font-size: 1.1em;
        }

        /* SVG Visuals */
        .visual-svg {
            width: 100%;
            max-width: 450px;
            height: 350px;
        }

        /* Slide-specific styles */
        .speed-indicator {
            font-size: 1.2em;
            font-weight: bold;
            fill: #e74c3c;
        }

        .trajectory-line {
            stroke-width: 4;
            stroke-dasharray: 5,5;
            opacity: 0.8;
        }

        .impact-effect {
            animation: pulse 2s ease-in-out infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 0.7; transform: scale(1); }
            50% { opacity: 1; transform: scale(1.1); }
        }

        .energy-flow {
            animation: energyMove 3s ease-in-out infinite;
        }

        @keyframes energyMove {
            0% { opacity: 0; }
            50% { opacity: 1; }
            100% { opacity: 0; }
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
        }

        .control-btn {
            padding: 12px 24px;
            background: linear-gradient(135deg, #3498db, #2980b9);
            color: white;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 14px;
            font-weight: bold;
            transition: all 0.3s ease;
        }

        .control-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(52,152,219,0.4);
        }

        .print-btn {
            background: linear-gradient(135deg, #27ae60, #2ecc71);
        }

        .print-btn:hover {
            box-shadow: 0 5px 15px rgba(46,204,113,0.4);
        }

        /* Print styles */
        @media print {
            body {
                background: white;
            }
            
            .slide-navigation,
            .controls {
                display: none;
            }
            
            .slide {
                display: block !important;
                page-break-after: always;
                box-shadow: none;
                border: 2px solid #ddd;
                margin-bottom: 0;
            }
            
            .slide:last-child {
                page-break-after: avoid;
            }
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            .slide-content {
                grid-template-columns: 1fr;
                gap: 20px;
            }
            
            .slide-visual {
                order: 1;
            }
            
            .slide-description {
                order: 2;
            }
            
            .slide-nav-btn {
                min-width: 100px;
                font-size: 12px;
                padding: 10px 15px;
            }
            
            .header h1 {
                font-size: 2em;
            }
        }
    </style>
</head>
<body>
    <div class="presentation-container">
        <div class="header">
            <h1>Down and Under 패턴</h1>
            <div class="subtitle">정면충돌 손상기전 교육자료</div>
            <div class="description">
                안전벨트 미착용 시 발생하는 특징적인 손상 메커니즘을 단계별로 분석합니다. 
                의료진 교육 및 교통안전 교육에 활용할 수 있도록 구성되었습니다.
            </div>
        </div>

        <div class="slide-navigation">
            <button class="slide-nav-btn active" onclick="showSlide(0)">1. 차량 급제동</button>
            <button class="slide-nav-btn" onclick="showSlide(1)">2. Down 움직임</button>
            <button class="slide-nav-btn" onclick="showSlide(2)">3. Under 움직임</button>
            <button class="slide-nav-btn" onclick="showSlide(3)">4. 무릎 충돌</button>
            <button class="slide-nav-btn" onclick="showSlide(4)">5. 에너지 전달</button>
            <button class="slide-nav-btn" onclick="showSlide(5)">6. 상체 손상</button>
        </div>

        <!-- 슬라이드 1: 차량 급제동 -->
        <div class="slide active" id="slide-0">
            <div class="slide-header">
                <div class="slide-title">1단계: 차량 급제동</div>
                <div class="slide-subtitle">Sudden Braking</div>
            </div>
            <div class="slide-content">
                <div class="slide-visual">
                    <svg class="visual-svg" viewBox="0 0 450 350">
                        <!-- Road -->
                        <rect x="0" y="250" width="450" height="100" fill="#4a4a4a"/>
                        <rect x="0" y="290" width="450" height="4" fill="#fff" opacity="0.8"/>
                        
                        <!-- Car exterior -->
                        <rect x="150" y="180" width="200" height="70" rx="15" fill="#2c5aa0"/>
                        <rect x="170" y="190" width="160" height="35" rx="8" fill="#87ceeb" opacity="0.7"/>
                        
                        <!-- Wheels -->
                        <circle cx="180" cy="260" r="20" fill="#1a1a1a"/>
                        <circle cx="320" cy="260" r="20" fill="#1a1a1a"/>
                        
                        <!-- Brake lights -->
                        <rect x="340" y="200" width="8" height="25" rx="4" fill="#ff0000" class="impact-effect"/>
                        
                        <!-- Tire marks -->
                        <rect x="50" y="265" width="120" height="4" fill="#333" opacity="0.8"/>
                        <rect x="50" y="275" width="120" height="4" fill="#333" opacity="0.8"/>
                        
                        <!-- Speed indicators -->
                        <text x="100" y="150" class="speed-indicator">60km/h</text>
                        <text x="300" y="150" class="speed-indicator">0km/h</text>
                        <line x1="140" y1="155" x2="260" y2="155" stroke="#e74c3c" stroke-width="3" marker-end="url(#arrowhead)"/>
                        
                        <!-- Inertia force arrow -->
                        <defs>
                            <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="0" refY="3.5" orient="auto">
                                <polygon points="0 0, 10 3.5, 0 7" fill="#e74c3c"/>
                            </marker>
                        </defs>
                        <line x1="250" y1="220" x2="320" y2="220" stroke="#e74c3c" stroke-width="4" marker-end="url(#arrowhead)"/>
                        <text x="280" y="240" fill="#e74c3c" font-weight="bold" font-size="14">관성력</text>
                        
                        <!-- Impact label -->
                        <text x="375" y="195" fill="#ff0000" font-weight="bold" font-size="12">브레이크등</text>
                        <text x="30" y="290" fill="#333" font-weight="bold" font-size="12">스키드 마크</text>
                    </svg>
                </div>
                <div class="slide-description">
                    <h3>물리적 상황 분석</h3>
                    <ul>
                        <li>차량이 정면 충돌하며 순간적으로 정지합니다 (예: 60km/h → 0)</li>
                        <li>브레이크등이 깜빡이고, 타이어는 스키드 마크를 남깁니다</li>
                        <li>이 순간, 탑승자에게 체중의 30~50배에 달하는 관성력이 작용합니다</li>
                        <li>차량은 멈추지만 승객의 몸은 계속 전진하려는 힘을 받습니다</li>
                    </ul>
                    <div class="key-points">
                        <h4>⚠️ 핵심 포인트</h4>
                        <p>뉴턴 제1법칙(관성의 법칙): 움직이던 물체는 외부 힘이 없으면 계속 움직이려 함</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- 슬라이드 2: Down 움직임 시작 -->
        <div class="slide" id="slide-1">
            <div class="slide-header">
                <div class="slide-title">2단계: Down 움직임 시작</div>
                <div class="slide-subtitle">하향 이동 시작</div>
            </div>
            <div class="slide-content">
                <div class="slide-visual">
                    <svg class="visual-svg" viewBox="0 0 450 350">
                        <!-- Car interior frame -->
                        <rect x="50" y="100" width="350" height="200" rx="15" fill="#333" fill-opacity="0.1" stroke="#555" stroke-width="2"/>
                        
                        <!-- Seat -->
                        <rect x="80" y="180" width="80" height="100" rx="10" fill="#2a2a2a"/>
                        <rect x="85" y="185" width="70" height="80" rx="8" fill="#444"/>
                        
                        <!-- Dashboard -->
                        <path d="M 300 120 Q 340 120 340 160 L 340 220 L 300 220 Z" fill="#2a2a2a"/>
                        
                        <!-- Passenger (initial position - dotted) -->
                        <g stroke="#bbb" stroke-width="2" fill="none" stroke-dasharray="3,3" opacity="0.4">
                            <circle cx="140" cy="140" r="15"/>
                            <rect x="125" y="155" width="30" height="50" rx="5"/>
                            <rect x="120" y="205" width="12" height="40" rx="6"/>
                            <rect x="148" y="205" width="12" height="40" rx="6"/>
                        </g>
                        
                        <!-- Passenger (moved position) -->
                        <g fill="#d4a574">
                            <circle cx="140" cy="160" r="15"/>
                            <rect x="125" y="175" width="30" height="50" rx="5" fill="#3498db"/>
                            <rect x="120" y="225" width="12" height="40" rx="6" fill="#1a365d"/>
                            <rect x="148" y="225" width="12" height="40" rx="6" fill="#1a365d"/>
                        </g>
                        
                        <!-- Down trajectory -->
                        <line x1="140" y1="100" x2="140" y2="200" stroke="#ffd700" stroke-width="4" class="trajectory-line"/>
                        <text x="150" y="150" fill="#ffd700" font-weight="bold" font-size="14">DOWN</text>
                        
                        <!-- Force arrows -->
                        <defs>
                            <marker id="yellowArrow" markerWidth="8" markerHeight="6" refX="0" refY="3" orient="auto">
                                <polygon points="0 0, 8 3, 0 6" fill="#ffd700"/>
                            </marker>
                        </defs>
                        <line x1="100" y1="140" x2="100" y2="180" stroke="#ffd700" stroke-width="3" marker-end="url(#yellowArrow)"/>
                        <text x="60" y="160" fill="#ffd700" font-weight="bold" font-size="12">중력+관성</text>
                        
                        <!-- No seatbelt indicator -->
                        <text x="200" y="140" fill="#e74c3c" font-weight="bold" font-size="16">⚠️ 안전벨트 미착용</text>
                    </svg>
                </div>
                <div class="slide-description">
                    <h3>Down 움직임 메커니즘</h3>
                    <ul>
                        <li>안전벨트를 착용하지 않은 승객은 상체를 제어하지 못합니다</li>
                        <li>중력과 관성이 함께 작용하며 몸이 아래로 미끄러지기 시작합니다</li>
                        <li>좌석 표면에서 승객이 하향으로 슬라이딩하는 현상이 발생합니다</li>
                        <li>노란색 수직 궤적선을 통해 Down 방향을 시각화합니다</li>
                    </ul>
                    <div class="key-points">
                        <h4>🔍 중요 관찰 포인트</h4>
                        <p>안전벨트가 있었다면 상체가 고정되어 이런 하향 이동이 방지되었을 것입니다</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- 슬라이드 3: Under 움직임 -->
        <div class="slide" id="slide-2">
            <div class="slide-header">
                <div class="slide-title">3단계: Under 움직임</div>
                <div class="slide-subtitle">대시보드 하부로 향하는 복합 궤적</div>
            </div>
            <div class="slide-content">
                <div class="slide-visual">
                    <svg class="visual-svg" viewBox="0 0 450 350">
                        <!-- Car interior -->
                        <rect x="50" y="100" width="350" height="200" rx="15" fill="#333" fill-opacity="0.1" stroke="#555" stroke-width="2"/>
                        
                        <!-- Dashboard -->
                        <path d="M 280 120 Q 320 120 320 160 L 320 220 L 280 220 Z" fill="#2a2a2a"/>
                        
                        <!-- Seat -->
                        <rect x="80" y="180" width="80" height="100" rx="10" fill="#2a2a2a"/>
                        
                        <!-- Passenger in forward position -->
                        <g fill="#d4a574">
                            <circle cx="180" cy="180" r="15"/>
                            <rect x="165" y="195" width="30" height="50" rx="5" fill="#3498db" transform="rotate(-10 180 220)"/>
                            <rect x="160" y="245" width="12" height="40" rx="6" fill="#1a365d" transform="rotate(-5 166 265)"/>
                            <rect x="188" y="245" width="12" height="40" rx="6" fill="#1a365d" transform="rotate(-5 194 265)"/>
                        </g>
                        
                        <!-- Complex trajectory (Down + Under) -->
                        <path d="M 140 140 Q 160 160 180 180 Q 220 200 260 240" stroke="#ffd700" stroke-width="4" fill="none" class="trajectory-line"/>
                        
                        <!-- Horizontal trajectory -->
                        <line x1="180" y1="240" x2="280" y2="240" stroke="#ff6600" stroke-width="4" class="trajectory-line"/>
                        
                        <!-- Labels -->
                        <text x="120" y="170" fill="#ffd700" font-weight="bold" font-size="12">DOWN</text>
                        <text x="220" y="230" fill="#ff6600" font-weight="bold" font-size="12">UNDER</text>
                        
                        <!-- Combined vector arrow -->
                        <defs>
                            <marker id="redArrow" markerWidth="10" markerHeight="7" refX="0" refY="3.5" orient="auto">
                                <polygon points="0 0, 10 3.5, 0 7" fill="#e74c3c"/>
                            </marker>
                        </defs>
                        <line x1="140" y1="140" x2="260" y2="240" stroke="#e74c3c" stroke-width="3" marker-end="url(#redArrow)"/>
                        
                        <!-- Formula -->
                        <text x="200" y="120" fill="#e74c3c" font-weight="bold" font-size="14">Down + Under = 복합 충돌 경로</text>
                        
                        <!-- Target area -->
                        <ellipse cx="290" cy="250" rx="30" ry="20" fill="#ff3333" opacity="0.3"/>
                        <text x="270" y="280" fill="#ff3333" font-weight="bold" font-size="12">충돌 목표점</text>
                    </svg>
                </div>
                <div class="slide-description">
                    <h3>Under 움직임 분석</h3>
                    <ul>
                        <li>관성에 의해 승객은 대시보드 아래 방향으로 이동합니다</li>
                        <li>Down(수직) + Under(수평)의 복합 궤적을 따라 움직입니다</li>
                        <li>수평 방향 궤적선으로 Under 경로를 강조합니다</li>
                        <li>최종적으로 대시보드 하단부를 향한 복합 벡터가 형성됩니다</li>
                    </ul>
                    <div class="key-points">
                        <h4>📐 물리학적 분석</h4>
                        <p><strong>수직 성분:</strong> 중력 + 좌석 미끄러짐<br>
                        <strong>수평 성분:</strong> 전방 관성력<br>
                        <strong>합성 벡터:</strong> 대시보드 하단부 충돌 경로</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- 슬라이드 4: 무릎 충돌 -->
        <div class="slide" id="slide-3">
            <div class="slide-header">
                <div class="slide-title">4단계: 무릎 충돌</div>
                <div class="slide-subtitle">Knee Impact - 1차 손상 발생</div>
            </div>
            <div class="slide-content">
                <div class="slide-visual">
                    <svg class="visual-svg" viewBox="0 0 450 350">
                        <!-- Car interior -->
                        <rect x="50" y="100" width="350" height="200" rx="15" fill="#333" fill-opacity="0.1" stroke="#555" stroke-width="2"/>
                        
                        <!-- Dashboard -->
                        <path d="M 280 120 Q 320 120 320 160 L 320 220 L 280 220 Z" fill="#2a2a2a"/>
                        
                        <!-- Passenger -->
                        <g fill="#d4a574">
                            <circle cx="200" cy="160" r="15"/>
                            <rect x="185" y="175" width="30" height="50" rx="5" fill="#3498db" transform="rotate(-15 200 200)"/>
                            <rect x="180" y="225" width="12" height="40" rx="6" fill="#1a365d" transform="rotate(-10 186 245)"/>
                            <rect x="208" y="225" width="12" height="40" rx="6" fill="#1a365d" transform="rotate(-10 214 245)"/>
                        </g>
                        
                        <!-- Knee impact points -->
                        <circle cx="290" cy="260" r="8" fill="#ff6b6b" class="impact-effect"/>
                        <circle cx="295" cy="265" r="8" fill="#ff6b6b" class="impact-effect"/>
                        
                        <!-- Impact shockwave -->
                        <circle cx="292" cy="262" r="20" fill="none" stroke="#ff3333" stroke-width="3" class="impact-effect"/>
                        <circle cx="292" cy="262" r="35" fill="none" stroke="#ff6666" stroke-width="2" opacity="0.6" class="impact-effect"/>
                        
                        <!-- Impact force arrow -->
                        <line x1="250" y1="262" x2="285" y2="262" stroke="#ff3333" stroke-width="5" marker-end="url(#redArrow)"/>
                        <text x="220" y="250" fill="#ff3333" font-weight="bold" font-size="14">충격력</text>
                        
                        <!-- Damage labels -->
                        <text x="310" y="250" fill="#e74c3c" font-weight="bold" font-size="12">1차 손상:</text>
                        <text x="310" y="265" fill="#e74c3c" font-size="11">• 슬개골 골절</text>
                        <text x="310" y="280" fill="#e74c3c" font-size="11">• 대퇴골 원위부 골절</text>
                        <text x="310" y="295" fill="#e74c3c" font-size="11">• 인대 손상</text>
                        
                        <!-- Force magnitude indicator -->
                        <rect x="350" y="140" width="80" height="25" fill="#ff3333" opacity="0.3" rx="5"/>
                        <text x="355" y="157" fill="#ff3333" font-weight="bold" font-size="12">고에너지 충격</text>
                    </svg>
                </div>
                <div class="slide-description">
                    <h3>무릎 충돌 메커니즘</h3>
                    <ul>
                        <li>무릎이 대시보드 하단부에 강하게 충돌합니다</li>
                        <li>슬개골 골절, 대퇴골 원위부 골절 등 하체 손상이 발생할 수 있습니다</li>
                        <li>충격 포인트에 시각적 파동 애니메이션이 강조됩니다</li>
                        <li>이때 발생하는 에너지는 다음 단계의 연쇄 손상을 유발합니다</li>
                    </ul>
                    <div class="key-points">
                        <h4>🦴 주요 손상 패턴</h4>
                        <p><strong>슬개골 골절:</strong> 직접적인 충돌 손상<br>
                        <strong>대퇴골 골절:</strong> 축 방향 압축력<br>
                        <strong>인대 손상:</strong> 과도한 굴곡과 압박</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- 슬라이드 5: 에너지 전달 -->
        <div class="slide" id="slide-4">
            <div class="slide-header">
                <div class="slide-title">5단계: 에너지 전달</div>
                <div class="slide-subtitle">Energy Transfer - 연쇄 손상</div>
            </div>
            <div class="slide-content">
                <div class="slide-visual">
                    <svg class="visual-svg" viewBox="0 0 450 350">
                        <!-- Car interior outline -->
                        <rect x="50" y="100" width="350" height="200" rx="15" fill="none" stroke="#555" stroke-width="1" opacity="0.3"/>
                        
                        <!-- Human figure (X-ray style) -->
                        <g fill="none" stroke="#87ceeb" stroke-width="2" opacity="0.6">
                            <circle cx="200" cy="140" r="18"/>
                            <rect x="180" y="160" width="40" height="60" rx="8"/>
                            <rect x="185" y="220" width="30" height="20" rx="5"/>
                        </g>
                        
                        <!-- Bone structure -->
                        <g stroke="#fff" stroke-width="4" opacity="0.8">
                            <!-- Femur bones -->
                            <line x1="188" y1="230" x2="188" y2="280" stroke="#ff6666"/>
                            <line x1="212" y1="230" x2="212" y2="280" stroke="#ff6666"/>
                            <!-- Tibia bones -->
                            <line x1="188" y1="280" x2="188" y2="320" stroke="#ff6666"/>
                            <line x1="212" y1="280" x2="212" y2="320" stroke="#ff6666"/>
                            <!-- Pelvis -->
                            <ellipse cx="200" cy="220" rx="25" ry="10" stroke="#ff6666"/>
                            <!-- Spine -->
                            <line x1="200" y1="160" x2="200" y2="210" stroke="#ff6666"/>
                        </g>
                        
                        <!-- Energy flow animation -->
                        <g class="energy-flow">
                            <circle cx="190" cy="320" r="4" fill="#ff3333"/>
                            <circle cx="190" cy="300" r="4" fill="#ff3333"/>
                            <circle cx="190" cy="280" r="4" fill="#ff3333"/>
                            <circle cx="190" cy="260" r="4" fill="#ff3333"/>
                            <circle cx="190" cy="240" r="4" fill="#ff3333"/>
                            <circle cx="190" cy="220" r="4" fill="#ff3333"/>
                            <circle cx="190" cy="200" r="4" fill="#ff3333"/>
                            <circle cx="190" cy="180" r="4" fill="#ff3333"/>
                        </g>
                        
                        <!-- Energy pathway arrows -->
                        <defs>
                            <marker id="energyArrow" markerWidth="8" markerHeight="6" refX="0" refY="3" orient="auto">
                                <polygon points="0 0, 8 3, 0 6" fill="#ff3333"/>
                            </marker>
                        </defs>
                        <line x1="210" y1="320" x2="210" y2="180" stroke="#ff3333" stroke-width="3" marker-end="url(#energyArrow)"/>
                        
                        <!-- Anatomical labels -->
                        <text x="250" y="180" fill="#e74c3c" font-weight="bold" font-size="12">척추</text>
                        <text x="250" y="220" fill="#e74c3c" font-weight="bold" font-size="12">골반</text>
                        <text x="250" y="260" fill="#e74c3c" font-weight="bold" font-size="12">대퇴골</text>
                        <text x="250" y="300" fill="#e74c3c" font-weight="bold" font-size="12">경골</text>
                        
                        <!-- Damage progression -->
                        <text x="280" y="140" fill="#ff3333" font-weight="bold" font-size="11">연쇄 손상:</text>
                        <text x="280" y="155" fill="#ff3333" font-size="10">• 고관절 탈구</text>
                        <text x="280" y="170" fill="#ff3333" font-size="10">• 비구 골절</text>
                        <text x="280" y="185" fill="#ff3333" font-size="10">• 요추 압박골절</text>
                    </svg>
                </div>
                <div class="slide-description">
                    <h3>에너지 전달 메커니즘</h3>
                    <ul>
                        <li>충격 에너지가 다리를 따라 골반, 척추로 전달됩니다</li>
                        <li>빨간색 펄스 선이 대퇴골에서 고관절, 척추 방향으로 이동합니다</li>
                        <li>고관절 탈구, 비구 골절, 요추 압박골절 위험이 있습니다</li>
                        <li>X-ray 스타일의 시각화로 뼈 구조와 에너지 경로를 명확히 표현합니다</li>
                    </ul>
                    <div class="key-points">
                        <h4>⚡ 연쇄 손상 패턴</h4>
                        <p><strong>1차:</strong> 무릎 → 대퇴골<br>
                        <strong>2차:</strong> 대퇴골 → 고관절<br>
                        <strong>3차:</strong> 골반 → 척추<br>
                        <strong>최종:</strong> 복부 장기 손상 가능</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- 슬라이드 6: 상체 손상 -->
        <div class="slide" id="slide-5">
            <div class="slide-header">
                <div class="slide-title">6단계: 상체 손상</div>
                <div class="slide-subtitle">Torso Forward - 2차 충돌</div>
            </div>
            <div class="slide-content">
                <div class="slide-visual">
                    <svg class="visual-svg" viewBox="0 0 450 350">
                        <!-- Car interior -->
                        <rect x="50" y="100" width="350" height="200" rx="15" fill="#333" fill-opacity="0.1" stroke="#555" stroke-width="2"/>
                        
                        <!-- Dashboard and steering wheel -->
                        <path d="M 280 120 Q 320 120 320 160 L 320 220 L 280 220 Z" fill="#2a2a2a"/>
                        <circle cx="300" cy="170" r="25" fill="#444" stroke="#666" stroke-width="3"/>
                        
                        <!-- Passenger (extreme forward position) -->
                        <g fill="#d4a574">
                            <circle cx="250" cy="150" r="15"/>
                            <rect x="235" y="165" width="30" height="50" rx="5" fill="#3498db" transform="rotate(-25 250 190)"/>
                            <rect x="230" y="175" width="10" height="30" rx="5" transform="rotate(-40 235 190)"/>
                            <rect x="255" y="175" width="10" height="30" rx="5" transform="rotate(10 260 190)"/>
                        </g>
                        
                        <!-- Legs stopped at dashboard -->
                        <rect x="285" y="230" width="12" height="50" rx="6" fill="#1a365d"/>
                        <rect x="305" y="230" width="12" height="50" rx="6" fill="#1a365d"/>
                        
                        <!-- Secondary impact zones -->
                        <circle cx="275" cy="170" r="6" fill="#ff6b6b" opacity="0.8" class="impact-effect"/>
                        <circle cx="270" cy="155" r="5" fill="#ff6b6b" opacity="0.8" class="impact-effect"/>
                        <circle cx="265" cy="175" r="5" fill="#ff6b6b" opacity="0.8" class="impact-effect"/>
                        
                        <!-- Risk zone -->
                        <ellipse cx="290" cy="165" rx="40" ry="25" fill="#ff3333" opacity="0.2"/>
                        
                        <!-- Motion arrows -->
                        <line x1="180" y1="150" x2="240" y2="150" stroke="#e74c3c" stroke-width="4" marker-end="url(#redArrow)"/>
                        <line x1="180" y1="190" x2="225" y2="190" stroke="#e74c3c" stroke-width="4" marker-end="url(#redArrow)"/>
                        
                        <!-- Body part labels with risk indicators -->
                        <text x="350" y="140" fill="#e74c3c" font-weight="bold" font-size="12">2차 손상 위험:</text>
                        <text x="350" y="160" fill="#e74c3c" font-size="11">🧠 두부 외상</text>
                        <text x="350" y="180" fill="#e74c3c" font-size="11">🦴 경추 손상</text>
                        <text x="350" y="200" fill="#e74c3c" font-size="11">🫁 흉부 압박</text>
                        <text x="350" y="220" fill="#e74c3c" font-size="11">🩸 내장 손상</text>
                        
                        <!-- Danger zone label -->
                        <text x="250" y="130" fill="#ff3333" font-weight="bold" font-size="14">위험 구역</text>
                        
                        <!-- Fixed lower body indicator -->
                        <text x="320" y="250" fill="#2c3e50" font-size="10">하체 고정</text>
                        <line x1="290" y1="255" x2="320" y2="255" stroke="#2c3e50" stroke-width="2"/>
                    </svg>
                </div>
                <div class="slide-description">
                    <h3>상체 손상 메커니즘</h3>
                    <ul>
                        <li>하체가 멈춘 후에도 상체는 관성으로 계속 앞으로 나아갑니다</li>
                        <li>스티어링 휠 또는 차량 내부와 2차 충돌이 발생할 수 있습니다</li>
                        <li>흉부, 경추, 두부에 심각한 손상이 생길 수 있습니다</li>
                        <li>상체와 하체 사이의 극심한 굴곡으로 척추 손상 위험이 증가합니다</li>
                    </ul>
                    <div class="key-points">
                        <h4>🚨 중대한 2차 손상</h4>
                        <p><strong>두부:</strong> 뇌진탕, 두개골 골절<br>
                        <strong>경추:</strong> Whiplash, 경추 탈구<br>
                        <strong>흉부:</strong> 늑골 골절, 폐 손상<br>
                        <strong>복부:</strong> 내장 파열, 대동맥 손상</p>
                    </div>
                </div>
            </div>
        </div>

        <div class="controls">
            <button class="control-btn" onclick="previousSlide()">◀ 이전</button>
            <button class="control-btn" onclick="nextSlide()">다음 ▶</button>
            <button class="control-btn" onclick="playSlideshow()">▶ 자동 재생</button>
            <button class="control-btn print-btn" onclick="printSlides()">🖨 인쇄용 출력</button>
        </div>
    </div>

    <script>
        let currentSlide = 0;
        let totalSlides = 6;
        let isPlaying = false;
        let playInterval;

        function showSlide(index) {
            // Hide all slides
            for (let i = 0; i < totalSlides; i++) {
                document.getElementById(`slide-${i}`).classList.remove('active');
                document.querySelectorAll('.slide-nav-btn')[i].classList.remove('active');
            }
            
            // Show selected slide
            document.getElementById(`slide-${index}`).classList.add('active');
            document.querySelectorAll('.slide-nav-btn')[index].classList.add('active');
            
            currentSlide = index;
        }

        function nextSlide() {
            const next = (currentSlide + 1) % totalSlides;
            showSlide(next);
        }

        function previousSlide() {
            const prev = (currentSlide - 1 + totalSlides) % totalSlides;
            showSlide(prev);
        }

        function playSlideshow() {
            if (isPlaying) {
                clearInterval(playInterval);
                isPlaying = false;
                document.querySelector('.control-btn:nth-child(3)').textContent = '▶ 자동 재생';
            } else {
                isPlaying = true;
                document.querySelector('.control-btn:nth-child(3)').textContent = '⏸ 정지';
                playInterval = setInterval(() => {
                    nextSlide();
                }, 4000);
            }
        }

        function printSlides() {
            window.print();
        }

        // Keyboard navigation
        document.addEventListener('keydown', function(e) {
            if (!isPlaying) {
                if (e.key === 'ArrowRight' || e.key === ' ') {
                    e.preventDefault();
                    nextSlide();
                } else if (e.key === 'ArrowLeft') {
                    e.preventDefault();
                    previousSlide();
                }
            }
            
            if (e.key === 'Escape') {
                if (isPlaying) {
                    playSlideshow();
                }
            }
        });

        // Initialize
        window.addEventListener('load', function() {
            showSlide(0);
        });

        // Touch navigation for mobile
        let touchStartX = 0;
        let touchEndX = 0;

        document.addEventListener('touchstart', function(e) {
            touchStartX = e.changedTouches[0].screenX;
        });

        document.addEventListener('touchend', function(e) {
            touchEndX = e.changedTouches[0].screenX;
            handleSwipe();
        });

        function handleSwipe() {
            if (touchEndX < touchStartX - 50) {
                nextSlide();
            }
            if (touchEndX > touchStartX + 50) {
                previousSlide();
            }
        }
    </script>
</body>
</html>
