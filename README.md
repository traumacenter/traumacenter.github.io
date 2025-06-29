<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AIS & ISS 간호업무 매뉴얼</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }
        
        .header {
            text-align: center;
            background: white;
            padding: 40px;
            border-radius: 20px;
            margin-bottom: 30px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
        }
        
        .header h1 {
            font-size: 2.5em;
            color: #2c3e50;
            margin-bottom: 15px;
            font-weight: 700;
        }
        
        .header p {
            font-size: 1.2em;
            color: #7f8c8d;
            margin-bottom: 10px;
        }
        
        .badge {
            display: inline-block;
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            padding: 8px 20px;
            border-radius: 25px;
            font-size: 0.9em;
            font-weight: 600;
        }
        
        .section {
            background: white;
            padding: 40px;
            border-radius: 20px;
            margin-bottom: 30px;
            box-shadow: 0 15px 30px rgba(0,0,0,0.1);
        }
        
        .section-title {
            font-size: 2em;
            color: #2c3e50;
            margin-bottom: 25px;
            text-align: center;
            position: relative;
            padding-bottom: 15px;
        }
        
        .section-title::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 80px;
            height: 4px;
            background: linear-gradient(135deg, #667eea, #764ba2);
            border-radius: 2px;
        }
        
        .ais-scale {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 40px;
        }
        
        .ais-card {
            background: linear-gradient(135deg, #f8f9ff 0%, #e8f2ff 100%);
            border: 3px solid transparent;
            border-radius: 15px;
            padding: 25px;
            text-align: center;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .ais-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 5px;
            background: var(--severity-color);
        }
        
        .ais-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(102, 126, 234, 0.2);
        }
        
        .ais-1 { --severity-color: #27ae60; }
        .ais-2 { --severity-color: #f1c40f; }
        .ais-3 { --severity-color: #e67e22; }
        .ais-4 { --severity-color: #e74c3c; }
        .ais-5 { --severity-color: #8e44ad; }
        .ais-6 { --severity-color: #2c3e50; }
        
        .ais-number {
            font-size: 3em;
            font-weight: bold;
            color: var(--severity-color);
            margin-bottom: 15px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }
        
        .ais-severity {
            font-size: 1.3em;
            font-weight: 600;
            color: #2c3e50;
            margin-bottom: 10px;
        }
        
        .ais-description {
            color: #7f8c8d;
            line-height: 1.6;
            font-size: 0.95em;
        }
        
        .body-regions {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
            gap: 25px;
            margin-bottom: 40px;
        }
        
        .region-card {
            background: linear-gradient(135deg, #fff5f5 0%, #ffe8e8 100%);
            border-left: 6px solid #e74c3c;
            border-radius: 15px;
            padding: 30px;
            transition: all 0.3s ease;
        }
        
        .region-card:hover {
            transform: translateX(10px);
            box-shadow: 0 15px 30px rgba(231, 76, 60, 0.2);
        }
        
        .region-number {
            display: inline-block;
            background: #e74c3c;
            color: white;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            text-align: center;
            line-height: 40px;
            font-weight: bold;
            font-size: 1.2em;
            margin-bottom: 15px;
        }
        
        .region-title {
            font-size: 1.4em;
            font-weight: 600;
            color: #2c3e50;
            margin-bottom: 10px;
        }
        
        .region-description {
            color: #7f8c8d;
            line-height: 1.6;
            margin-bottom: 15px;
        }
        
        .region-examples {
            background: rgba(231, 76, 60, 0.1);
            padding: 15px;
            border-radius: 10px;
            border-left: 4px solid #e74c3c;
        }
        
        .region-examples strong {
            color: #e74c3c;
            display: block;
            margin-bottom: 8px;
        }
        
        .calculation-section {
            background: linear-gradient(135deg, #e8f5e8 0%, #f0fdf4 100%);
            border-radius: 20px;
            padding: 40px;
            margin-bottom: 30px;
        }
        
        .calculation-steps {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .step-card {
            background: white;
            padding: 25px;
            border-radius: 15px;
            text-align: center;
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
            position: relative;
        }
        
        .step-number {
            background: #27ae60;
            color: white;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 1.2em;
            margin: 0 auto 15px;
        }
        
        .step-title {
            font-weight: 600;
            color: #2c3e50;
            margin-bottom: 10px;
        }
        
        .step-description {
            color: #7f8c8d;
            font-size: 0.9em;
            line-height: 1.5;
        }
        
        .example-calculation {
            background: white;
            padding: 30px;
            border-radius: 15px;
            border: 3px solid #27ae60;
        }
        
        .example-title {
            font-size: 1.5em;
            color: #27ae60;
            font-weight: 600;
            margin-bottom: 20px;
            text-align: center;
        }
        
        .calculation-table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }
        
        .calculation-table th,
        .calculation-table td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #ecf0f1;
        }
        
        .calculation-table th {
            background: #27ae60;
            color: white;
            font-weight: 600;
        }
        
        .calculation-result {
            background: linear-gradient(135deg, #27ae60, #2ecc71);
            color: white;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            font-size: 1.3em;
            font-weight: bold;
        }
        
        .severity-interpretation {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 30px;
        }
        
        .severity-card {
            padding: 20px;
            border-radius: 12px;
            text-align: center;
            font-weight: 600;
            color: white;
        }
        
        .severity-mild { background: linear-gradient(135deg, #27ae60, #2ecc71); }
        .severity-moderate { background: linear-gradient(135deg, #f39c12, #e67e22); }
        .severity-severe { background: linear-gradient(135deg, #e74c3c, #c0392b); }
        .severity-critical { background: linear-gradient(135deg, #8e44ad, #9b59b6); }
        
        .nursing-tips {
            background: linear-gradient(135deg, #fff9e6 0%, #fef5e7 100%);
            border-left: 6px solid #f39c12;
            padding: 30px;
            border-radius: 15px;
            margin-top: 30px;
        }
        
        .nursing-tips h3 {
            color: #d68910;
            font-size: 1.3em;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
        }
        
        .nursing-tips h3::before {
            content: "💡";
            margin-right: 10px;
            font-size: 1.2em;
        }
        
        .tips-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
        }
        
        .tip-item {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }
        
        .tip-title {
            font-weight: 600;
            color: #d68910;
            margin-bottom: 10px;
        }
        
        .tip-content {
            color: #7f8c8d;
            line-height: 1.6;
            font-size: 0.9em;
        }
        
        @media (max-width: 768px) {
            .container {
                padding: 10px;
            }
            
            .header {
                padding: 20px;
            }
            
            .section {
                padding: 20px;
            }
            
            .header h1 {
                font-size: 2em;
            }
            
            .ais-scale {
                grid-template-columns: 1fr;
            }
            
            .body-regions {
                grid-template-columns: 1fr;
            }
        }
        
        .footer {
            text-align: center;
            padding: 40px;
            background: rgba(255,255,255,0.9);
            border-radius: 20px;
            margin-top: 30px;
        }
        
        .footer p {
            color: #7f8c8d;
            margin-bottom: 10px;
        }
        
        .footer .badge {
            background: linear-gradient(135deg, #e74c3c, #c0392b);
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <div class="header">
            <h1>🏥 AIS & ISS 간호업무 매뉴얼</h1>
            <p>학생간호사 및 신규간호사를 위한 외상 중증도 평가 가이드</p>
            <div class="badge">근거기반 간호실무</div>
        </div>

        <!-- AIS Section -->
        <div class="section">
            <h2 class="section-title">📊 AIS (Abbreviated Injury Scale) 점수 체계</h2>
            <p style="text-align: center; margin-bottom: 30px; color: #7f8c8d; font-size: 1.1em;">
                각각의 개별 손상에 대해 1점부터 6점까지 중증도를 평가하는 척도
            </p>
            
            <div class="ais-scale">
                <div class="ais-card ais-1">
                    <div class="ais-number">1</div>
                    <div class="ais-severity">경미한 손상</div>
                    <div class="ais-description">
                        생명에 위험이 없는 가벼운 손상<br>
                        <strong>예시:</strong> 표면적 찰과상, 경미한 타박상
                    </div>
                </div>
                
                <div class="ais-card ais-2">
                    <div class="ais-number">2</div>
                    <div class="ais-severity">중등도 손상</div>
                    <div class="ais-description">
                        심각하지만 생명에 즉각적 위험 없음<br>
                        <strong>예시:</strong> 단순 골절, 2도 화상
                    </div>
                </div>
                
                <div class="ais-card ais-3">
                    <div class="ais-number">3</div>
                    <div class="ais-severity">심각한 손상</div>
                    <div class="ais-description">
                        생명에 위험은 없지만 심각한 손상<br>
                        <strong>예시:</strong> 복합 골절, 중등도 뇌진탕
                    </div>
                </div>
                
                <div class="ais-card ais-4">
                    <div class="ais-number">4</div>
                    <div class="ais-severity">중증 손상</div>
                    <div class="ais-description">
                        생명을 위협할 수 있는 심각한 손상<br>
                        <strong>예시:</strong> 대량 출혈, 장기 파열
                    </div>
                </div>
                
                <div class="ais-card ais-5">
                    <div class="ais-number">5</div>
                    <div class="ais-severity">위험한 손상</div>
                    <div class="ais-description">
                        생존이 불확실한 매우 위험한 손상<br>
                        <strong>예시:</strong> 대동맥 파열, 중증 뇌손상
                    </div>
                </div>
                
                <div class="ais-card ais-6">
                    <div class="ais-number">6</div>
                    <div class="ais-severity">최대 중증</div>
                    <div class="ais-description">
                        현재 의학으로 치료 불가능한 치명적 손상<br>
                        <strong>예시:</strong> 뇌간 완전 손상
                    </div>
                </div>
            </div>
        </div>

        <!-- ISS Body Regions -->
        <div class="section">
            <h2 class="section-title">🫁 ISS 6개 신체 영역</h2>
            <p style="text-align: center; margin-bottom: 30px; color: #7f8c8d; font-size: 1.1em;">
                인체를 6개 영역으로 나누어 각 영역에서 가장 높은 AIS 점수를 사용
            </p>
            
            <div class="body-regions">
                <div class="region-card">
                    <div class="region-number">1</div>
                    <div class="region-title">머리 또는 목 (Head or Neck)</div>
                    <div class="region-description">뇌, 두개골, 경추 척추를 포함</div>
                    <div class="region-examples">
                        <strong>주요 손상:</strong>
                        뇌출혈, 두개골 골절, 경추 손상, 뇌진탕
                    </div>
                </div>
                
                <div class="region-card">
                    <div class="region-number">2</div>
                    <div class="region-title">얼굴 (Face)</div>
                    <div class="region-description">안면골, 코, 입, 눈, 귀를 포함</div>
                    <div class="region-examples">
                        <strong>주요 손상:</strong>
                        안면골 골절, 치아 손상, 안구 손상, 비골 골절
                    </div>
                </div>
                
                <div class="region-card">
                    <div class="region-number">3</div>
                    <div class="region-title">흉부 (Chest)</div>
                    <div class="region-description">흉곽, 폐, 심장, 흉추, 횡격막을 포함</div>
                    <div class="region-examples">
                        <strong>주요 손상:</strong>
                        기흉, 혈흉, 늑골 골절, 심장 손상, 대동맥 손상
                    </div>
                </div>
                
                <div class="region-card">
                    <div class="region-number">4</div>
                    <div class="region-title">복부 또는 골반 내용물</div>
                    <div class="region-description">복부 장기, 요추 척추를 포함</div>
                    <div class="region-examples">
                        <strong>주요 손상:</strong>
                        간 파열, 비장 손상, 장 손상, 신장 손상
                    </div>
                </div>
                
                <div class="region-card">
                    <div class="region-number">5</div>
                    <div class="region-title">사지 또는 골반 골격</div>
                    <div class="region-description">팔, 다리, 골반뼈를 포함</div>
                    <div class="region-examples">
                        <strong>주요 손상:</strong>
                        사지 골절, 골반 골절, 혈관 손상, 절단
                    </div>
                </div>
                
                <div class="region-card">
                    <div class="region-number">6</div>
                    <div class="region-title">체표 (External)</div>
                    <div class="region-description">피부, 피하조직을 포함</div>
                    <div class="region-examples">
                        <strong>주요 손상:</strong>
                        화상, 열상, 압궤상, 광범위한 피부 손상
                    </div>
                </div>
            </div>
        </div>

        <!-- ISS Calculation -->
        <div class="calculation-section">
            <h2 class="section-title">🧮 ISS 계산법</h2>
            
            <div class="calculation-steps">
                <div class="step-card">
                    <div class="step-number">1</div>
                    <div class="step-title">손상 분류</div>
                    <div class="step-description">환자의 모든 손상을 6개 영역별로 분류</div>
                </div>
                
                <div class="step-card">
                    <div class="step-number">2</div>
                    <div class="step-title">AIS 점수 결정</div>
                    <div class="step-description">각 영역에서 가장 높은 AIS 점수 찾기</div>
                </div>
                
                <div class="step-card">
                    <div class="step-number">3</div>
                    <div class="step-title">상위 3개 선택</div>
                    <div class="step-description">가장 높은 점수 3개 영역 선택</div>
                </div>
                
                <div class="step-card">
                    <div class="step-number">4</div>
                    <div class="step-title">제곱 후 합산</div>
                    <div class="step-description">각각을 제곱한 후 모두 더하기</div>
                </div>
            </div>
            
            <div class="example-calculation">
                <div class="example-title">📋 실제 계산 예시</div>
                
                <table class="calculation-table">
                    <thead>
                        <tr>
                            <th>신체 영역</th>
                            <th>손상 내용</th>
                            <th>AIS 점수</th>
                            <th>제곱값</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>머리/목</td>
                            <td>중등도 뇌진탕</td>
                            <td>4</td>
                            <td>16</td>
                        </tr>
                        <tr>
                            <td>흉부</td>
                            <td>늑골 골절 (다발성)</td>
                            <td>3</td>
                            <td>9</td>
                        </tr>
                        <tr>
                            <td>복부</td>
                            <td>비장 열상</td>
                            <td>2</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>사지</td>
                            <td>대퇴골 골절</td>
                            <td>3</td>
                            <td>-</td>
                        </tr>
                    </tbody>
                </table>
                
                <div class="calculation-result">
                    ISS = 16 + 9 + 4 = 29점
                </div>
            </div>
        </div>

        <!-- Severity Interpretation -->
        <div class="section">
            <h2 class="section-title">📈 ISS 점수별 중증도 해석</h2>
            
            <div class="severity-interpretation">
                <div class="severity-card severity-mild">
                    <div style="font-size: 1.2em; margin-bottom: 10px;">💚 1-8점</div>
                    <div>경증 외상</div>
                    <div style="font-size: 0.9em; margin-top: 8px;">외래 치료 가능</div>
                </div>
                
                <div class="severity-card severity-moderate">
                    <div style="font-size: 1.2em; margin-bottom: 10px;">💛 9-15점</div>
                    <div>중등도 외상</div>
                    <div style="font-size: 0.9em; margin-top: 8px;">입원 치료 필요</div>
                </div>
                
                <div class="severity-card severity-severe">
                    <div style="font-size: 1.2em; margin-bottom: 10px;">🧡 16-24점</div>
                    <div>중증 외상</div>
                    <div style="font-size: 0.9em; margin-top: 8px;">집중치료 고려</div>
                </div>
                
                <div class="severity-card severity-critical">
                    <div style="font-size: 1.2em; margin-bottom: 10px;">💜 25점 이상</div>
                    <div>최중증 외상</div>
                    <div style="font-size: 0.9em; margin-top: 8px;">집중치료실 필수</div>
                </div>
            </div>
        </div>

        <!-- Nursing Tips -->
        <div class="nursing-tips">
            <h3>간호실무 핵심 포인트</h3>
            
            <div class="tips-grid">
                <div class="tip-item">
                    <div class="tip-title">🚨 응급실에서</div>
                    <div class="tip-content">
                        • 환자 도착 즉시 1차 평가로 AIS 점수 산정<br>
                        • ISS 계산으로 치료 우선순위 결정<br>
                        • 중증도에 따른 병실 배정 결정
                    </div>
                </div>
                
                <div class="tip-item">
                    <div class="tip-title">🏥 병동에서</div>
                    <div class="tip-content">
                        • 환자 인수인계 시 ISS 점수 확인<br>
                        • 합병증 발생 위험도 예측<br>
                        • 간호 계획 수립의 기준점 활용
                    </div>
                </div>
                
                <div class="tip-item">
                    <div class="tip-title">🔍 평가 시 주의사항</div>
                    <div class="tip-content">
                        • 모든 손상을 놓치지 않고 평가<br>
                        • 지연성 손상 가능성 염두<br>
                        • 정확한 해부학적 지식 필요
                    </div>
                </div>
                
                <div class="tip-item">
                    <div class="tip-title">📊 해석 시 주의사항</div>
                    <div class="tip-content">
                        • 점수가 낮다고 방심 금물<br>
                        • 단일 장기 손상도 치명적일 수 있음<br>
                        • 환자 상태는 지속적으로 변화
                    </div>
                </div>
            </div>
        </div>

        <!-- Footer -->
        <div class="footer">
            <p><strong>기억하세요:</strong> 숫자 뒤에는 소중한 생명이 있습니다.</p>
            <p>점수는 도구일 뿐, 환자 중심의 전인적 간호가 가장 중요합니다.</p>
            <div class="badge">환자안전 최우선</div>
        </div>
    </div>
</body>
</html>
