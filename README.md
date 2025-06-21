<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>외상의 기전 학습 가이드</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.1);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #ff6b6b, #ee5a24);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
        }

        .main-content {
            padding: 40px;
        }

        .mechanism-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 25px;
            margin-bottom: 40px;
        }

        .mechanism-card {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 3px solid transparent;
            position: relative;
            overflow: hidden;
        }

        .mechanism-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(0,0,0,0.1);
        }

        .mechanism-card.active {
            border-color: #ff6b6b;
            background: #fff5f5;
        }

        .card-icon {
            width: 60px;
            height: 60px;
            background: #ff6b6b;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 20px;
            font-size: 24px;
        }

        .direct-impact { background: #ff6b6b; }
        .indirect-impact { background: #4ecdc4; }
        .deceleration { background: #45b7d1; }
        .rotational { background: #f9ca24; }
        .compression { background: #6c5ce7; }

        .card-title {
            font-size: 1.4em;
            font-weight: bold;
            margin-bottom: 15px;
            color: #2c3e50;
        }

        .card-description {
            color: #666;
            line-height: 1.6;
            margin-bottom: 15px;
        }

        .detail-panel {
            display: none;
            background: white;
            border-radius: 15px;
            padding: 30px;
            margin-top: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            border-left: 5px solid #ff6b6b;
        }

        .detail-panel.active {
            display: block;
            animation: slideDown 0.3s ease;
        }

        @keyframes slideDown {
            from { opacity: 0; transform: translateY(-20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .detail-header {
            display: flex;
            align-items: center;
            margin-bottom: 25px;
        }

        .detail-icon {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 20px;
            font-size: 20px;
        }

        .detail-title {
            font-size: 1.8em;
            font-weight: bold;
            color: #2c3e50;
        }

        .detail-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-bottom: 25px;
        }

        .clinical-example {
            background: #e8f5e8;
            padding: 20px;
            border-radius: 10px;
            border-left: 4px solid #27ae60;
        }

        .nursing-care {
            background: #fff3cd;
            padding: 20px;
            border-radius: 10px;
            border-left: 4px solid #ffc107;
        }

        .section-title {
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 15px;
            font-size: 1.1em;
        }

        .example-list {
            list-style: none;
        }

        .example-list li {
            margin-bottom: 8px;
            padding-left: 20px;
            position: relative;
        }

        .example-list li:before {
            content: "▶";
            position: absolute;
            left: 0;
            color: #27ae60;
            font-weight: bold;
        }

        .priority-box {
            background: #ffebee;
            border: 2px solid #e91e63;
            border-radius: 10px;
            padding: 20px;
            margin-top: 20px;
        }

        .priority-title {
            color: #e91e63;
            font-weight: bold;
            margin-bottom: 10px;
            font-size: 1.1em;
        }

        .traffic-accident {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border-radius: 15px;
            padding: 30px;
            margin: 40px 0;
        }

        .traffic-title {
            font-size: 1.8em;
            margin-bottom: 20px;
            text-align: center;
        }

        .accident-types {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
        }

        .accident-card {
            background: rgba(255,255,255,0.1);
            border-radius: 10px;
            padding: 20px;
            backdrop-filter: blur(10px);
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .accident-card:hover {
            background: rgba(255,255,255,0.2);
            transform: scale(1.05);
        }

        .accident-card.active {
            background: rgba(255,255,255,0.3);
            border: 2px solid #fff;
            transform: scale(1.05);
        }

        .accident-icon {
            font-size: 2em;
            margin-bottom: 15px;
            text-align: center;
        }

        .reference-note {
            background: #f8f9fa;
            border: 1px solid #dee2e6;
            border-radius: 10px;
            padding: 20px;
            margin-top: 30px;
            text-align: center;
            color: #6c757d;
        }

        .btn-reset {
            background: #6c757d;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 20px auto;
            display: block;
        }

        .btn-reset:hover {
            background: #495057;
            transform: scale(1.05);
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🩺 외상의 기전 학습 가이드</h1>
            <p>학생간호사 & 신규간호사를 위한 인터렉티브 매뉴얼</p>
        </div>

        <div class="main-content">
            <h2 style="color: #2c3e50; margin-bottom: 30px; text-align: center;">💡 외상 기전별 분류 (카드를 클릭하세요!)</h2>
            
            <div class="mechanism-grid">
                <div class="mechanism-card" data-mechanism="direct">
                    <div class="card-icon direct-impact">🎯</div>
                    <div class="card-title">직접 충격 (Direct Impact)</div>
                    <div class="card-description">물체가 직접 신체에 충돌하여 발생하는 손상</div>
                </div>

                <div class="mechanism-card" data-mechanism="indirect">
                    <div class="card-icon indirect-impact">🌊</div>
                    <div class="card-title">간접 충격 (Indirect Impact)</div>
                    <div class="card-description">충격이 골격계를 따라 전달되어 다른 부위에서 발생하는 손상</div>
                </div>

                <div class="mechanism-card" data-mechanism="deceleration">
                    <div class="card-icon deceleration">⚡</div>
                    <div class="card-title">감속 손상 (Deceleration)</div>
                    <div class="card-description">급작스러운 속도 변화로 인한 내부 장기 손상</div>
                </div>

                <div class="mechanism-card" data-mechanism="rotational">
                    <div class="card-icon rotational">🌀</div>
                    <div class="card-title">회전 손상 (Rotational)</div>
                    <div class="card-description">회전력에 의한 나선형 골절과 인대 파열</div>
                </div>

                <div class="mechanism-card" data-mechanism="compression">
                    <div class="card-icon compression">🗜️</div>
                    <div class="card-title">압축 손상 (Compression)</div>
                    <div class="card-description">두 개의 힘이 서로 반대 방향에서 조직을 압축</div>
                </div>
            </div>

            <!-- 상세 정보 패널들 -->
            <div id="direct-detail" class="detail-panel">
                <div class="detail-header">
                    <div class="detail-icon direct-impact">🎯</div>
                    <div class="detail-title">직접 충격 (Direct Impact)</div>
                </div>
                <div class="detail-content">
                    <div class="clinical-example">
                        <div class="section-title">🏥 임상 예시</div>
                        <ul class="example-list">
                            <li><strong>야구공에 맞은 경우:</strong> 타박상, 혈종, 국소 골절</li>
                            <li><strong>주먹으로 맞은 경우:</strong> 안와 골절, 비골 골절</li>
                            <li><strong>벽에 머리를 부딪힌 경우:</strong> 두피 열상, 뇌진탕</li>
                            <li><strong>무릎을 바닥에 부딪힌 경우:</strong> 슬개골 골절, 연부조직 손상</li>
                        </ul>
                    </div>
                    <div class="nursing-care">
                        <div class="section-title">👩‍⚕️ 간호 주의점</div>
                        <ul class="example-list">
                            <li>충돌 부위 주변의 숨겨진 손상 확인</li>
                            <li>신경혈관 손상 여부 사정</li>
                            <li>부종과 통증 정도 평가</li>
                            <li>국소 손상과 에너지 전달 경로 동시 사정</li>
                        </ul>
                    </div>
                </div>
                <div class="priority-box">
                    <div class="priority-title">🚨 간호 우선순위</div>
                    충돌 지점에서 최대 에너지가 집중되므로 국소적 손상이 주요 특징입니다.
                </div>
            </div>

            <div id="indirect-detail" class="detail-panel">
                <div class="detail-header">
                    <div class="detail-icon indirect-impact">🌊</div>
                    <div class="detail-title">간접 충격 (Indirect Impact)</div>
                </div>
                <div class="detail-content">
                    <div class="clinical-example">
                        <div class="section-title">🏥 임상 예시</div>
                        <ul class="example-list">
                            <li><strong>발목 골절 시:</strong> 에너지가 상행하여 경골/비골 골절, 무릎 인대 손상</li>
                            <li><strong>손목 골절 시:</strong> 요골/척골 골절, 팔꿈치 관절 손상</li>
                            <li><strong>꼬리뼈 낙상 시:</strong> 척추 압박골절, 경추 과신전 손상</li>
                            <li><strong>발뒤꿈치 착지 시:</strong> 종골 골절 → 경골/비골 골절</li>
                        </ul>
                    </div>
                    <div class="nursing-care">
                        <div class="section-title">👩‍⚕️ 간호 주의점</div>
                        <ul class="example-list">
                            <li>명백한 손상 부위뿐만 아니라 에너지 전달 경로상의 모든 부위 사정</li>
                            <li>통증 호소가 없어도 체계적 검사 필요</li>
                            <li>지연성 증상 발현 가능성 설명</li>
                        </ul>
                    </div>
                </div>
                <div class="priority-box">
                    <div class="priority-title">🚨 간호 우선순위</div>
                    에너지 전달 경로 상의 모든 부위 사정이 최우선입니다.
                </div>
            </div>

            <div id="deceleration-detail" class="detail-panel">
                <div class="detail-header">
                    <div class="detail-icon deceleration">⚡</div>
                    <div class="detail-title">감속 손상 (Deceleration Injury)</div>
                </div>
                <div class="detail-content">
                    <div class="clinical-example">
                        <div class="section-title">🏥 임상 예시</div>
                        <ul class="example-list">
                            <li><strong>대동맥 파열:</strong> 대동맥궁 ligamentum arteriosum 부근</li>
                            <li><strong>심장 좌상:</strong> 심장이 흉벽에 충돌</li>
                            <li><strong>미만성 축삭 손상:</strong> 뇌조직의 다른 밀도로 인한 전단손상</li>
                            <li><strong>경막하 혈종:</strong> 뇌와 두개골의 속도 차이</li>
                        </ul>
                    </div>
                    <div class="nursing-care">
                        <div class="section-title">👩‍⚕️ 간호 주의점</div>
                        <ul class="example-list">
                            <li>초기에는 증상이 미미할 수 있음</li>
                            <li>24-48시간 집중 관찰 필요</li>
                            <li>활력징후 변화와 신경학적 변화 모니터링</li>
                            <li>내출혈 징후 관찰 (혈압 저하, 빈맥, 복부 팽만)</li>
                        </ul>
                    </div>
                </div>
                <div class="priority-box">
                    <div class="priority-title">🚨 간호 우선순위</div>
                    내출혈 모니터링이 최우선입니다!
                </div>
            </div>

            <div id="rotational-detail" class="detail-panel">
                <div class="detail-header">
                    <div class="detail-icon rotational">🌀</div>
                    <div class="detail-title">회전 손상 (Rotational Injury)</div>
                </div>
                <div class="detail-content">
                    <div class="clinical-example">
                        <div class="section-title">🏥 임상 예시</div>
                        <ul class="example-list">
                            <li><strong>나선형 골절:</strong> 경골/비골의 나선형 골절 (스키 사고)</li>
                            <li><strong>인대 파열:</strong> 전십자인대(ACL) 파열</li>
                            <li><strong>미만성 축삭 손상:</strong> 뇌간과 대뇌의 회전 속도 차이</li>
                            <li><strong>장간막 파열:</strong> 소장이 회전하면서 장간막 혈관 손상</li>
                        </ul>
                    </div>
                    <div class="nursing-care">
                        <div class="section-title">👩‍⚕️ 간호 주의점</div>
                        <ul class="example-list">
                            <li>신경혈관 손상 동반 가능성 높음</li>
                            <li>원위부 감각, 운동, 순환 상태 정기적 사정</li>
                            <li>구획증후군 발생 주의</li>
                            <li>지연성 신경 증상 관찰</li>
                        </ul>
                    </div>
                </div>
                <div class="priority-box">
                    <div class="priority-title">🚨 간호 우선순위</div>
                    신경혈관 상태 집중 관찰이 필수입니다!
                </div>
            </div>

            <div id="compression-detail" class="detail-panel">
                <div class="detail-header">
                    <div class="detail-icon compression">🗜️</div>
                    <div class="detail-title">압축 손상 (Compression Injury)</div>
                </div>
                <div class="detail-content">
                    <div class="clinical-example">
                        <div class="section-title">🏥 임상 예시</div>
                        <ul class="example-list">
                            <li><strong>척추 압박골절:</strong> 수직 압축력으로 인한 척추체 붕괴</li>
                            <li><strong>플레일 흉부:</strong> 여러 개의 늑골이 두 곳 이상에서 골절</li>
                            <li><strong>간 파열:</strong> 우상복부 압박으로 인한 간 피막 파열</li>
                            <li><strong>구획증후군:</strong> 근육 구획 내 압력 증가</li>
                        </ul>
                    </div>
                    <div class="nursing-care">
                        <div class="section-title">👩‍⚕️ 간호 주의점</div>
                        <ul class="example-list">
                            <li>압박 해제 후에도 지속적인 관찰 필요</li>
                            <li>재관류 손상 가능성</li>
                            <li>횡문근융해증 발생 주의</li>
                            <li><strong>전해질 불균형 모니터링 (특히 칼륨 수치)</strong></li>
                        </ul>
                    </div>
                </div>
                <div class="priority-box">
                    <div class="priority-title">🚨 간호 우선순위</div>
                    구획증후군과 재관류 손상 주의! 전해질 모니터링 중요!
                </div>
            </div>

            <!-- 교통사고 손상 기전 -->
            <div class="traffic-accident">
                <div class="traffic-title">🚗 교통사고 손상 기전</div>
                <div class="accident-types">
                    <div class="accident-card" data-traffic="frontal">
                        <div class="accident-icon">🎯</div>
                        <h3>정면 충돌</h3>
                        <p>Down & Under / Up & Over 패턴</p>
                    </div>
                    <div class="accident-card" data-traffic="side">
                        <div class="accident-icon">↔️</div>
                        <h3>측면 충돌</h3>
                        <p>T-bone 사고, 측면 압박</p>
                    </div>
                    <div class="accident-card" data-traffic="rear">
                        <div class="accident-icon">⬅️</div>
                        <h3>후면 충돌</h3>
                        <p>편타성 손상 (Whiplash)</p>
                    </div>
                    <div class="accident-card" data-traffic="rollover">
                        <div class="accident-icon">🔄</div>
                        <h3>전복 사고</h3>
                        <p>다발성 손상, 예측 불가</p>
                    </div>
                </div>
            </div>

            <!-- 교통사고 상세 패널들 -->
            <div id="frontal-detail" class="detail-panel">
                <div class="detail-header">
                    <div class="detail-icon" style="background: #e74c3c;">🎯</div>
                    <div class="detail-title">정면 충돌 (Frontal Collision)</div>
                </div>
                
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 25px; margin-bottom: 25px;">
                    <div style="background: #fff3cd; padding: 20px; border-radius: 10px; border-left: 4px solid #ffc107;">
                        <div class="section-title">🔽 Down and Under 패턴</div>
                        <p style="margin-bottom: 15px;"><strong>발생순서:</strong></p>
                        <ul class="example-list">
                            <li>차량 급정지 → 몸이 앞으로 밀림</li>
                            <li>무릎이 대시보드 충돌</li>
                            <li>에너지가 다리를 따라 전달</li>
                            <li>상체가 계속 앞으로 움직임</li>
                        </ul>
                        <p style="margin-top: 15px;"><strong>예상손상:</strong></p>
                        <ul class="example-list">
                            <li>슬개골 골절, 무릎 인대 파열</li>
                            <li>대퇴골 간부 골절</li>
                            <li>고관절 후방 탈구</li>
                            <li>골반골 골절</li>
                        </ul>
                    </div>
                    
                    <div style="background: #e8f5e8; padding: 20px; border-radius: 10px; border-left: 4px solid #27ae60;">
                        <div class="section-title">🔼 Up and Over 패턴</div>
                        <p style="margin-bottom: 15px;"><strong>발생순서:</strong></p>
                        <ul class="example-list">
                            <li>차량 급정지 → 관성으로 몸이 앞으로</li>
                            <li>머리/가슴이 충돌</li>
                            <li>목이 과도하게 굽어짐</li>
                            <li>내장기관도 앞으로 밀림</li>
                        </ul>
                        <p style="margin-top: 15px;"><strong>예상손상:</strong></p>
                        <ul class="example-list">
                            <li>두개골 골절, 뇌출혈, 뇌진탕</li>
                            <li>경추 골절, 척수 손상</li>
                            <li>늑골 골절, 기흉, 혈흉</li>
                            <li>간, 비장, 신장 파열</li>
                        </ul>
                    </div>
                </div>
                
                <div class="priority-box">
                    <div class="priority-title">🚨 간호 우선순위</div>
                    Down & Under: 골반 골절로 인한 내출혈 주의 | Up & Over: 기도 확보 및 경추 고정이 최우선!
                </div>
            </div>

            <div id="side-detail" class="detail-panel">
                <div class="detail-header">
                    <div class="detail-icon" style="background: #9b59b6;">↔️</div>
                    <div class="detail-title">측면 충돌 (Side Impact)</div>
                </div>
                <div class="detail-content">
                    <div class="clinical-example">
                        <div class="section-title">🏥 발생 과정</div>
                        <ul class="example-list">
                            <li><strong>1단계:</strong> 측면 충격</li>
                            <li><strong>2단계:</strong> 차체 변형</li>
                            <li><strong>3단계:</strong> 승객이 충돌 방향으로 밀림</li>
                            <li><strong>4단계:</strong> 머리, 어깨, 골반이 차체와 충돌</li>
                            <li><strong>5단계:</strong> 반대편으로 튕겨나감</li>
                        </ul>
                    </div>
                    <div class="nursing-care">
                        <div class="section-title">👩‍⚕️ 예상 손상</div>
                        <ul class="example-list">
                            <li><strong>직접 충격:</strong> 측두골 골절, 뇌출혈</li>
                            <li><strong>어깨:</strong> 쇄골 골절, 어깨 탈구</li>
                            <li><strong>흉부:</strong> 늑골 골절(여러 개), 기흉</li>
                            <li><strong>골반:</strong> 골반골 골절, 고관절 탈구</li>
                            <li><strong>내장기관:</strong> 간/비장 파열, 신장 손상</li>
                        </ul>
                    </div>
                </div>
                <div class="priority-box">
                    <div class="priority-title">🚨 왜 측면 충돌이 위험한가?</div>
                    측면은 정면/후면보다 보호 구조가 약하고, 문과 승객 사이 거리가 가까워(30-40cm) 충격을 흡수할 공간이 부족합니다.
                </div>
            </div>

            <div id="rear-detail" class="detail-panel">
                <div class="detail-header">
                    <div class="detail-icon" style="background: #3498db;">⬅️</div>
                    <div class="detail-title">후면 충돌 (Rear Impact) - Whiplash</div>
                </div>
                <div class="detail-content">
                    <div class="clinical-example">
                        <div class="section-title">🏥 편타성 손상 과정</div>
                        <ul class="example-list">
                            <li><strong>1단계:</strong> 후면 충격 → 차량이 앞으로 밀림</li>
                            <li><strong>2단계:</strong> 좌석이 승객을 앞으로 밀어냄</li>
                            <li><strong>3단계:</strong> 머리만 뒤에 남아있음</li>
                            <li><strong>4단계:</strong> 목이 과도하게 뒤로 젖혀짐 (과신전)</li>
                            <li><strong>5단계:</strong> 반동으로 앞으로 꺾임 (과굴곡)</li>
                        </ul>
                    </div>
                    <div class="nursing-care">
                        <div class="section-title">👩‍⚕️ 경추 손상 특징</div>
                        <ul class="example-list">
                            <li><strong>과신전 단계:</strong> 목 앞쪽 근육, 인대 손상</li>
                            <li><strong>과굴곡 단계:</strong> 목 뒤쪽 근육, 인대 손상</li>
                            <li><strong>기타 손상:</strong> 요추 손상, 흉부 손상</li>
                            <li><strong>심리적:</strong> PTSD, 운전 공포증</li>
                        </ul>
                    </div>
                </div>
                <div class="priority-box">
                    <div class="priority-title">🚨 왜 목 손상이 주로 생길까?</div>
                    머리 무게 약 4-5kg(볼링공 정도)을 가느다란 목 구조가 지탱하므로, 갑작스러운 가속-감속에 가장 취약합니다.
                </div>
            </div>

            <div id="rollover-detail" class="detail-panel">
                <div class="detail-header">
                    <div class="detail-icon" style="background: #e67e22;">🔄</div>
                    <div class="detail-title">전복 사고 (Rollover)</div>
                </div>
                <div class="detail-content">
                    <div class="clinical-example">
                        <div class="section-title">🏥 발생 과정</div>
                        <ul class="example-list">
                            <li><strong>1단계:</strong> 고속 주행</li>
                            <li><strong>2단계:</strong> 급작스런 방향 전환</li>
                            <li><strong>3단계:</strong> 차량 전복</li>
                            <li><strong>4단계:</strong> 여러 방향 충격</li>
                            <li><strong>5단계:</strong> 승객이 차 안에서 튕겨다님</li>
                            <li><strong>6단계:</strong> 다양한 부위에서 반복적 충격</li>
                        </ul>
                    </div>
                    <div class="nursing-care">
                        <div class="section-title">👩‍⚕️ 예측 불가능한 손상</div>
                        <ul class="example-list">
                            <li><strong>머리:</strong> 여러 번 부딪혀 심각한 뇌외상</li>
                            <li><strong>척추:</strong> 다양한 방향의 힘으로 복잡한 골절</li>
                            <li><strong>사지:</strong> 여러 부위의 골절과 탈구</li>
                            <li><strong>내장기관:</strong> 전방위적 손상</li>
                        </ul>
                    </div>
                </div>
                <div class="priority-box">
                    <div class="priority-title">🚨 전복 사고가 가장 위험한 이유</div>
                    예측 불가능한 충격 방향, 반복적 충돌, 다양한 손상 기전이 혼재하며, 차량 변형으로 구조가 지연됩니다.
                </div>
            </div>

            <button class="btn-reset" onclick="resetAllCards()">🔄 모든 카드 닫기</button>

            <div class="reference-note">
                <strong>📚 Reference:</strong> 본 자료는 근거기반 간호실무 가이드라인을 바탕으로 제작되었습니다.<br>
                외상 환자 간호 시 항상 ABCDE 접근법을 우선으로 하며, 의료진과의 협력을 통해 안전한 간호를 제공하세요.
            </div>
        </div>
    </div>

    <script>
        function showMechanismDetail(mechanism) {
            // 모든 카드와 패널 비활성화
            document.querySelectorAll('.mechanism-card').forEach(card => {
                card.classList.remove('active');
            });
            document.querySelectorAll('.detail-panel').forEach(panel => {
                panel.classList.remove('active');
            });

            // 선택된 카드와 패널 활성화
            document.querySelector(`[data-mechanism="${mechanism}"]`).classList.add('active');
            document.getElementById(`${mechanism}-detail`).classList.add('active');
            
            // 부드럽게 스크롤
            document.getElementById(`${mechanism}-detail`).scrollIntoView({
                behavior: 'smooth',
                block: 'start'
            });
        }

        function showTrafficDetail(traffic) {
            // 모든 카드와 패널 비활성화
            document.querySelectorAll('.mechanism-card').forEach(card => {
                card.classList.remove('active');
            });
            document.querySelectorAll('.accident-card').forEach(card => {
                card.classList.remove('active');
            });
            document.querySelectorAll('.detail-panel').forEach(panel => {
                panel.classList.remove('active');
            });

            // 선택된 교통사고 카드와 패널 활성화
            document.querySelector(`[data-traffic="${traffic}"]`).classList.add('active');
            document.getElementById(`${traffic}-detail`).classList.add('active');
            
            // 부드럽게 스크롤
            document.getElementById(`${traffic}-detail`).scrollIntoView({
                behavior: 'smooth',
                block: 'start'
            });
        }

        function resetAllCards() {
            document.querySelectorAll('.mechanism-card').forEach(card => {
                card.classList.remove('active');
            });
            document.querySelectorAll('.accident-card').forEach(card => {
                card.classList.remove('active');
            });
            document.querySelectorAll('.detail-panel').forEach(panel => {
                panel.classList.remove('active');
            });
        }

        // 카드 클릭 이벤트 리스너
        document.querySelectorAll('.mechanism-card').forEach(card => {
            card.addEventListener('click', () => {
                const mechanism = card.getAttribute('data-mechanism');
                showMechanismDetail(mechanism);
            });
        });

        // 교통사고 카드 클릭 이벤트 리스너
        document.querySelectorAll('.accident-card').forEach(card => {
            card.addEventListener('click', () => {
                const traffic = card.getAttribute('data-traffic');
                showTrafficDetail(traffic);
            });
        });

        // 초기 로딩 애니메이션
        window.addEventListener('load', () => {
            document.querySelectorAll('.mechanism-card').forEach((card, index) => {
                setTimeout(() => {
                    card.style.opacity = '0';
                    card.style.transform = 'translateY(50px)';
                    setTimeout(() => {
                        card.style.transition = 'all 0.5s ease';
                        card.style.opacity = '1';
                        card.style.transform = 'translateY(0)';
                    }, 100);
                }, index * 100);
            });

            // 교통사고 카드 애니메이션
            document.querySelectorAll('.accident-card').forEach((card, index) => {
                setTimeout(() => {
                    card.style.opacity = '0';
                    card.style.transform = 'translateX(-50px)';
                    setTimeout(() => {
                        card.style.transition = 'all 0.5s ease';
                        card.style.opacity = '1';
                        card.style.transform = 'translateX(0)';
                    }, 100);
                }, (index * 150) + 500);
            });
        });
    </script>
</body>
</html>
