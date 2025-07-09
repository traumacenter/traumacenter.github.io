<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CRRT 간호 AI 시스템</title>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700&display=swap" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Noto Sans KR', sans-serif;
            background-color: #f5f7fa;
            color: #333;
        }
        
        /* 헤더 스타일 */
        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 1rem 2rem;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        
        .header-content {
            max-width: 1400px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 1rem;
        }
        
        .logo i {
            font-size: 2rem;
        }
        
        .user-info {
            display: flex;
            align-items: center;
            gap: 1rem;
        }
        
        /* 메인 컨테이너 */
        .container {
            max-width: 1400px;
            margin: 2rem auto;
            padding: 0 1rem;
        }
        
        /* 그리드 레이아웃 */
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
        }
        
        /* 카드 기본 스타일 */
        .card {
            background: white;
            border-radius: 12px;
            padding: 1.5rem;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            transition: transform 0.2s, box-shadow 0.2s;
        }
        
        .card:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 16px rgba(0,0,0,0.12);
        }
        
        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1rem;
        }
        
        .card-title {
            font-size: 1.25rem;
            font-weight: 500;
            color: #2c3e50;
        }
        
        /* 위험도 표시 스타일 */
        .risk-indicator {
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            padding: 0.5rem 1rem;
            border-radius: 24px;
            font-weight: 500;
            font-size: 0.9rem;
        }
        
        .risk-high {
            background-color: #ffebee;
            color: #c62828;
        }
        
        .risk-medium {
            background-color: #fff3e0;
            color: #ef6c00;
        }
        
        .risk-low {
            background-color: #e8f5e9;
            color: #2e7d32;
        }
        
        /* 환자 리스크 카드 */
        .patient-card {
            border-left: 4px solid;
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }
        
        .patient-card.high-risk {
            border-left-color: #f44336;
        }
        
        .patient-card.medium-risk {
            border-left-color: #ff9800;
        }
        
        .patient-card.low-risk {
            border-left-color: #4caf50;
        }
        
        .patient-info {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .patient-name {
            font-size: 1.1rem;
            font-weight: 500;
        }
        
        .metrics {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 1rem;
            margin-top: 1rem;
        }
        
        .metric {
            text-align: center;
            padding: 0.75rem;
            background-color: #f8f9fa;
            border-radius: 8px;
        }
        
        .metric-value {
            font-size: 1.5rem;
            font-weight: 600;
            color: #667eea;
        }
        
        .metric-label {
            font-size: 0.85rem;
            color: #666;
            margin-top: 0.25rem;
        }
        
        /* 실시간 모니터링 차트 */
        .chart-container {
            height: 300px;
            position: relative;
            margin-top: 1rem;
            padding: 1rem;
            background-color: #fafafa;
            border-radius: 8px;
        }
        
        .chart-placeholder {
            height: 100%;
            background: linear-gradient(to right, #f0f0f0 0%, #e0e0e0 50%, #f0f0f0 100%);
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #999;
        }
        
        /* 우선순위 리스트 */
        .priority-list {
            list-style: none;
        }
        
        .priority-item {
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 1rem;
            border-bottom: 1px solid #eee;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        
        .priority-item:hover {
            background-color: #f8f9fa;
        }
        
        .priority-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
        }
        
        .priority-high .priority-icon {
            background-color: #ffebee;
            color: #f44336;
        }
        
        .priority-medium .priority-icon {
            background-color: #fff3e0;
            color: #ff9800;
        }
        
        .priority-low .priority-icon {
            background-color: #e8f5e9;
            color: #4caf50;
        }
        
        .priority-content {
            flex: 1;
        }
        
        .priority-title {
            font-weight: 500;
            margin-bottom: 0.25rem;
        }
        
        .priority-desc {
            font-size: 0.9rem;
            color: #666;
        }
        
        .priority-time {
            font-size: 0.85rem;
            color: #999;
        }
        
        /* 알림 패널 */
        .alert-item {
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 1rem;
            background-color: #f8f9fa;
            border-radius: 8px;
            margin-bottom: 0.75rem;
            border-left: 3px solid;
        }
        
        .alert-critical {
            border-left-color: #f44336;
            background-color: #ffebee;
        }
        
        .alert-warning {
            border-left-color: #ff9800;
            background-color: #fff3e0;
        }
        
        .alert-info {
            border-left-color: #2196f3;
            background-color: #e3f2fd;
        }
        
        .alert-icon {
            font-size: 1.5rem;
        }
        
        .alert-content {
            flex: 1;
        }
        
        .alert-title {
            font-weight: 500;
            margin-bottom: 0.25rem;
        }
        
        .alert-time {
            font-size: 0.85rem;
            color: #666;
        }
        
        /* 액션 버튼 */
        .btn {
            padding: 0.5rem 1.5rem;
            border: none;
            border-radius: 6px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 0.9rem;
        }
        
        .btn-primary {
            background-color: #667eea;
            color: white;
        }
        
        .btn-primary:hover {
            background-color: #5a67d8;
        }
        
        .btn-secondary {
            background-color: #e0e0e0;
            color: #333;
        }
        
        .btn-secondary:hover {
            background-color: #d0d0d0;
        }
        
        /* 빠른 액션 플로팅 버튼 */
        .fab {
            position: fixed;
            bottom: 2rem;
            right: 2rem;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background-color: #667eea;
            color: white;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
            transition: all 0.3s;
        }
        
        .fab:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(102, 126, 234, 0.5);
        }
        
        /* 반응형 디자인 */
        @media (max-width: 768px) {
            .dashboard-grid {
                grid-template-columns: 1fr;
            }
            
            .metrics {
                grid-template-columns: 1fr;
            }
            
            .header-content {
                flex-direction: column;
                gap: 1rem;
                text-align: center;
            }
        }
    </style>
</head>
<body>
    <!-- 헤더 -->
    <header class="header">
        <div class="header-content">
            <div class="logo">
                <i class="fas fa-heartbeat"></i>
                <div>
                    <h1 style="font-size: 1.5rem; margin-bottom: 0.25rem;">CRRT 간호 AI 시스템</h1>
                    <p style="font-size: 0.9rem; opacity: 0.9;">실시간 환자 모니터링 대시보드</p>
                </div>
            </div>
            <div class="user-info">
                <span><i class="fas fa-clock"></i> 2024.03.15 14:30</span>
                <span><i class="fas fa-user"></i> 김간호 (TICU-A)</span>
                <button class="btn btn-secondary" style="background: rgba(255,255,255,0.2); color: white;">
                    <i class="fas fa-sign-out-alt"></i> 로그아웃
                </button>
            </div>
        </div>
    </header>

    <!-- 메인 컨테이너 -->
    <div class="container">
        <!-- 요약 정보 -->
        <div class="dashboard-grid">
            <!-- 전체 환자 현황 -->
            <div class="card">
                <div class="card-header">
                    <h2 class="card-title">TICU 환자 현황</h2>
                    <button class="btn btn-primary">상세보기</button>
                </div>
                <div class="metrics">
                    <div class="metric">
                        <div class="metric-value">12</div>
                        <div class="metric-label">총 환자수</div>
                    </div>
                    <div class="metric">
                        <div class="metric-value" style="color: #f44336;">3</div>
                        <div class="metric-label">고위험군</div>
                    </div>
                    <div class="metric">
                        <div class="metric-value" style="color: #ff9800;">5</div>
                        <div class="metric-label">중간위험군</div>
                    </div>
                </div>
            </div>

            <!-- AI 예측 요약 -->
            <div class="card">
                <div class="card-header">
                    <h2 class="card-title">AI 예측 알림</h2>
                    <span class="risk-indicator risk-high">
                        <i class="fas fa-exclamation-triangle"></i> 즉시 확인 필요
                    </span>
                </div>
                <div style="display: flex; flex-direction: column; gap: 0.75rem;">
                    <div style="display: flex; align-items: center; gap: 0.5rem;">
                        <i class="fas fa-bacteria" style="color: #f44336;"></i>
                        <span>CAUTI 고위험: BED1 박OO (87% 확률)</span>
                    </div>
                    <div style="display: flex; align-items: center; gap: 0.5rem;">
                        <i class="fas fa-tint" style="color: #ff9800;"></i>
                        <span>CLABSI 주의: BED5 이OO (65% 확률)</span>
                    </div>
                    <div style="display: flex; align-items: center; gap: 0.5rem;">
                        <i class="fas fa-chart-line" style="color: #2196f3;"></i>
                        <span>상태 악화 예측: BED8 김OO (24시간 내)</span>
                    </div>
                </div>
            </div>

            <!-- 급성신손상 위험 예측 카드 추가 -->
            <div class="card">
                <div class="card-header">
                    <h2 class="card-title">
                        <i class="fas fa-kidneys" style="color: #9c27b0;"></i> AKI 위험도 분석
                    </h2>
                    <select class="btn btn-secondary" id="akiPatientSelect" style="font-size: 0.9rem;">
                        <option value="bed1">BED1 박OO</option>
                        <option value="bed5">BED5 이OO</option>
                        <option value="bed8">BED8 김OO</option>
                    </select>
                </div>
                <!-- 환자 정보 표시 -->
                <div id="akiPatientInfo" style="background-color: #f8f9fa; padding: 0.75rem; border-radius: 6px; margin-bottom: 1rem; font-size: 0.9rem;">
                    <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 0.5rem;">
                        <span><strong>BED1</strong> 박OO (65세/남)</span>
                        <span>ISS: 28점</span>
                        <span>입원: 72시간 경과</span>
                        <span>진단: 다발성 외상</span>
                    </div>
                </div>
                <div style="margin-bottom: 1rem;">
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 0.5rem;">
                        <span style="font-weight: 500;">48시간 내 AKI 발생 예측</span>
                        <span id="akiRiskScore" style="font-size: 1.5rem; font-weight: 700; color: #f44336;">72%</span>
                    </div>
                    <div style="background: linear-gradient(to right, #4caf50 0%, #ffeb3b 50%, #f44336 100%); height: 8px; border-radius: 4px; position: relative;">
                        <div id="akiRiskIndicator" style="position: absolute; left: 72%; top: -4px; width: 16px; height: 16px; background: #f44336; border-radius: 50%; border: 2px solid white;"></div>
                    </div>
                </div>
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 0.75rem; margin-bottom: 1rem;">
                    <div style="background-color: #f5f5f5; padding: 0.75rem; border-radius: 6px;">
                        <div style="font-size: 0.85rem; color: #666;">Cr 변화율</div>
                        <div id="crChange" style="font-size: 1.25rem; font-weight: 600; color: #f44336;">↑ 45%</div>
                    </div>
                    <div style="background-color: #f5f5f5; padding: 0.75rem; border-radius: 6px;">
                        <div style="font-size: 0.85rem; color: #666;">요량 (ml/hr)</div>
                        <div id="urineOutput" style="font-size: 1.25rem; font-weight: 600; color: #ff9800;">28</div>
                    </div>
                </div>
                <div id="akiRiskFactors" style="background-color: #f3e5f5; padding: 1rem; border-radius: 8px;">
                    <div style="font-weight: 500; margin-bottom: 0.5rem; color: #7b1fa2;">
                        <i class="fas fa-exclamation-circle"></i> 주요 위험 요인
                    </div>
                    <ul style="margin: 0; padding-left: 1.5rem; font-size: 0.9rem;">
                        <li>대량수혈: pRBC 14 units (4hr)</li>
                        <li>횡문근융해증: CK 8,500</li>
                        <li>저혈압: MAP < 65 (2hr)</li>
                        <li>조영제 사용: 복부 CT (3회)</li>
                    </ul>
                </div>
                <div style="margin-top: 1rem; display: flex; gap: 0.5rem;">
                    <button class="btn btn-primary" style="flex: 1; font-size: 0.9rem;">
                        <i class="fas fa-clipboard-check"></i> 예방 프로토콜
                    </button>
                    <button class="btn btn-secondary" style="flex: 1; font-size: 0.9rem;">
                        <i class="fas fa-chart-bar"></i> 상세 분석
                    </button>
                </div>
            </div>
        </div>

        <!-- 환자별 상세 정보 -->
        <h2 style="margin: 2rem 0 1rem; font-size: 1.5rem; color: #2c3e50;">고위험 환자 모니터링</h2>
        <div class="dashboard-grid">
            <!-- 환자 1 -->
            <div class="card patient-card high-risk">
                <div class="patient-info">
                    <div>
                        <div class="patient-name">BED1 박OO (65세/남)</div>
                        <div style="font-size: 0.9rem; color: #666; margin-top: 0.25rem;">CRRT 3일차</div>
                    </div>
                    <span class="risk-indicator risk-high">
                        <i class="fas fa-exclamation-circle"></i> 고위험
                    </span>
                </div>
                <div class="metrics">
                    <div class="metric">
                        <div class="metric-value" style="color: #f44336;">87%</div>
                        <div class="metric-label">감염 위험도</div>
                    </div>
                    <div class="metric">
                        <div class="metric-value">4.5</div>
                        <div class="metric-label">간호 집중도</div>
                    </div>
                    <div class="metric">
                        <div class="metric-value">2h</div>
                        <div class="metric-label">다음 모니터링</div>
                    </div>
                </div>
                <div style="background-color: #ffebee; padding: 1rem; border-radius: 8px; margin-top: 1rem;">
                    <strong style="color: #c62828;">⚠️ 주의사항:</strong>
                    <ul style="margin-top: 0.5rem; margin-left: 1.5rem; font-size: 0.9rem;">
                        <li>도뇨관 삽입 부위 발적 관찰됨</li>
                        <li>체온 38.2°C로 상승 추세</li>
                        <li>WBC 15,000으로 증가</li>
                    </ul>
                </div>
            </div>

            <!-- 환자 2 -->
            <div class="card patient-card medium-risk">
                <div class="patient-info">
                    <div>
                        <div class="patient-name">BED5 이OO (72세/여)</div>
                        <div style="font-size: 0.9rem; color: #666; margin-top: 0.25rem;">CRRT 5일차</div>
                    </div>
                    <span class="risk-indicator risk-medium">
                        <i class="fas fa-exclamation"></i> 중간위험
                    </span>
                </div>
                <div class="metrics">
                    <div class="metric">
                        <div class="metric-value" style="color: #ff9800;">65%</div>
                        <div class="metric-label">감염 위험도</div>
                    </div>
                    <div class="metric">
                        <div class="metric-value">3.2</div>
                        <div class="metric-label">간호 집중도</div>
                    </div>
                    <div class="metric">
                        <div class="metric-value">4h</div>
                        <div class="metric-label">다음 모니터링</div>
                    </div>
                </div>
                <div style="background-color: #fff3e0; padding: 1rem; border-radius: 8px; margin-top: 1rem;">
                    <strong style="color: #ef6c00;">📋 모니터링 포인트:</strong>
                    <ul style="margin-top: 0.5rem; margin-left: 1.5rem; font-size: 0.9rem;">
                        <li>중심정맥관 드레싱 교체 필요</li>
                        <li>필터 응고 징후 관찰</li>
                        <li>전해질 불균형 모니터링</li>
                    </ul>
                </div>
            </div>
        </div>

        <!-- 실시간 모니터링 차트 -->
        <div class="card" style="margin-top: 2rem;">
            <div class="card-header">
                <h2 class="card-title">실시간 생체 신호 모니터링</h2>
                <div style="display: flex; gap: 1rem;">
                    <select class="btn btn-secondary" id="patientSelect">
                        <option value="bed1">BED1 박OO (고위험)</option>
                        <option value="bed5">BED5 이OO (중간위험)</option>
                        <option value="bed8">BED8 김OO (저위험)</option>
                    </select>
                    <button class="btn btn-primary">
                        <i class="fas fa-download"></i> 데이터 내보내기
                    </button>
                </div>
            </div>
            <div class="chart-container">
                <canvas id="vitalSignsChart"></canvas>
            </div>
            <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 1rem; margin-top: 1rem;">
                <div class="metric">
                    <div class="metric-value" id="currentBP">120/80</div>
                    <div class="metric-label">혈압 (mmHg)</div>
                </div>
                <div class="metric">
                    <div class="metric-value" id="currentHR">75</div>
                    <div class="metric-label">맥박 (bpm)</div>
                </div>
                <div class="metric">
                    <div class="metric-value" id="currentRR">16</div>
                    <div class="metric-label">호흡수 (/min)</div>
                </div>
                <div class="metric">
                    <div class="metric-value" id="currentTemp">36.8</div>
                    <div class="metric-label">체온 (°C)</div>
                </div>
            </div>
        </div>

        <!-- 간호 업무 우선순위 -->
        <div class="dashboard-grid" style="margin-top: 2rem;">
            <div class="card">
                <div class="card-header">
                    <h2 class="card-title">간호 업무 우선순위</h2>
                    <span style="font-size: 0.9rem; color: #666;">14:30 기준</span>
                </div>
                <ul class="priority-list">
                    <li class="priority-item priority-high">
                        <div class="priority-icon">
                            <i class="fas fa-exclamation"></i>
                        </div>
                        <div class="priority-content">
                            <div class="priority-title">BED1 도뇨관 교체</div>
                            <div class="priority-desc">감염 위험 증가로 즉시 교체 필요</div>
                        </div>
                        <div class="priority-time">즉시</div>
                    </li>
                    <li class="priority-item priority-medium">
                        <div class="priority-icon">
                            <i class="fas fa-clock"></i>
                        </div>
                        <div class="priority-content">
                            <div class="priority-title">BED5 필터 점검</div>
                            <div class="priority-desc">응고 징후 확인 필요</div>
                        </div>
                        <div class="priority-time">30분 내</div>
                    </li>
                    <li class="priority-item priority-low">
                        <div class="priority-icon">
                            <i class="fas fa-check"></i>
                        </div>
                        <div class="priority-content">
                            <div class="priority-title">BED8 정기 모니터링</div>
                            <div class="priority-desc">활력징후 및 I/O 확인</div>
                        </div>
                        <div class="priority-time">1시간 내</div>
                    </li>
                </ul>
            </div>

            <!-- 실시간 알림 -->
            <div class="card">
                <div class="card-header">
                    <h2 class="card-title">실시간 알림</h2>
                    <button class="btn btn-secondary">
                        <i class="fas fa-bell"></i> 알림 설정
                    </button>
                </div>
                <div>
                    <div class="alert-item alert-critical">
                        <div class="alert-icon" style="color: #f44336;">
                            <i class="fas fa-exclamation-triangle"></i>
                        </div>
                        <div class="alert-content">
                            <div class="alert-title">BED1 CRRT 알람 발생</div>
                            <div class="alert-time">2분 전</div>
                        </div>
                    </div>
                    <div class="alert-item alert-warning">
                        <div class="alert-icon" style="color: #ff9800;">
                            <i class="fas fa-thermometer-half"></i>
                        </div>
                        <div class="alert-content">
                            <div class="alert-title">BED5 체온 상승 (37.8°C)</div>
                            <div class="alert-time">15분 전</div>
                        </div>
                    </div>
                    <div class="alert-item alert-info">
                        <div class="alert-icon" style="color: #2196f3;">
                            <i class="fas fa-info-circle"></i>
                        </div>
                        <div class="alert-content">
                            <div class="alert-title">AI 예측 모델 업데이트 완료</div>
                            <div class="alert-time">30분 전</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- 빠른 액션 플로팅 버튼 -->
    <button class="fab">
        <i class="fas fa-plus"></i>
    </button>

    <script>
        // 실시간 시간 업데이트
        function updateTime() {
            const now = new Date();
            const timeString = now.toLocaleString('ko-KR', { 
                year: 'numeric', 
                month: '2-digit', 
                day: '2-digit', 
                hour: '2-digit', 
                minute: '2-digit' 
            });
            document.querySelector('.fa-clock').nextSibling.textContent = ' ' + timeString;
        }
        
        // 1분마다 시간 업데이트
        setInterval(updateTime, 60000);
        
        // 페이지 로드 후 실행
        document.addEventListener('DOMContentLoaded', function() {
        
        // 바이탈 사인 차트 설정
        const ctx = document.getElementById('vitalSignsChart').getContext('2d');
        const vitalSignsChart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: [],
                datasets: [{
                    label: '수축기 혈압',
                    data: [],
                    borderColor: '#f44336',
                    backgroundColor: 'rgba(244, 67, 54, 0.1)',
                    tension: 0.4,
                    yAxisID: 'y-bp',
                }, {
                    label: '이완기 혈압',
                    data: [],
                    borderColor: '#ff9800',
                    backgroundColor: 'rgba(255, 152, 0, 0.1)',
                    tension: 0.4,
                    yAxisID: 'y-bp',
                }, {
                    label: '맥박',
                    data: [],
                    borderColor: '#4caf50',
                    backgroundColor: 'rgba(76, 175, 80, 0.1)',
                    tension: 0.4,
                    yAxisID: 'y-hr',
                }, {
                    label: '호흡수',
                    data: [],
                    borderColor: '#2196f3',
                    backgroundColor: 'rgba(33, 150, 243, 0.1)',
                    tension: 0.4,
                    yAxisID: 'y-rr',
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                interaction: {
                    mode: 'index',
                    intersect: false,
                },
                plugins: {
                    legend: {
                        position: 'bottom',
                    },
                    title: {
                        display: true,
                        text: '최근 2시간 바이탈 사인 추이'
                    }
                },
                scales: {
                    x: {
                        display: true,
                        title: {
                            display: true,
                            text: '시간'
                        }
                    },
                    'y-bp': {
                        type: 'linear',
                        display: true,
                        position: 'left',
                        title: {
                            display: true,
                            text: '혈압 (mmHg)'
                        },
                        min: 40,
                        max: 180,
                    },
                    'y-hr': {
                        type: 'linear',
                        display: true,
                        position: 'right',
                        title: {
                            display: true,
                            text: '맥박 (bpm)'
                        },
                        min: 40,
                        max: 140,
                        grid: {
                            drawOnChartArea: false,
                        },
                    },
                    'y-rr': {
                        type: 'linear',
                        display: true,
                        position: 'right',
                        title: {
                            display: true,
                            text: '호흡수 (/min)'
                        },
                        min: 8,
                        max: 30,
                        grid: {
                            drawOnChartArea: false,
                        },
                    }
                }
            }
        });
        
        // 초기 데이터 생성 (최근 2시간)
        const now = new Date();
        for (let i = 24; i >= 0; i--) {
            const time = new Date(now - i * 5 * 60 * 1000);
            vitalSignsChart.data.labels.push(time.toLocaleTimeString('ko-KR', { hour: '2-digit', minute: '2-digit' }));
            
            // 실제같은 바이탈 사인 데이터 생성
            vitalSignsChart.data.datasets[0].data.push(115 + Math.random() * 15); // 수축기
            vitalSignsChart.data.datasets[1].data.push(70 + Math.random() * 10);  // 이완기
            vitalSignsChart.data.datasets[2].data.push(70 + Math.random() * 20);  // 맥박
            vitalSignsChart.data.datasets[3].data.push(14 + Math.random() * 6);   // 호흡수
        }
        vitalSignsChart.update();
        
        // 5초마다 새로운 데이터 추가
        setInterval(() => {
            const time = new Date();
            vitalSignsChart.data.labels.push(time.toLocaleTimeString('ko-KR', { hour: '2-digit', minute: '2-digit' }));
            vitalSignsChart.data.labels.shift();
            
            // 새로운 바이탈 사인 추가
            const newSBP = 115 + Math.random() * 15;
            const newDBP = 70 + Math.random() * 10;
            const newHR = 70 + Math.random() * 20;
            const newRR = 14 + Math.random() * 6;
            const newTemp = 36.5 + Math.random() * 0.8;
            
            vitalSignsChart.data.datasets[0].data.push(newSBP);
            vitalSignsChart.data.datasets[0].data.shift();
            vitalSignsChart.data.datasets[1].data.push(newDBP);
            vitalSignsChart.data.datasets[1].data.shift();
            vitalSignsChart.data.datasets[2].data.push(newHR);
            vitalSignsChart.data.datasets[2].data.shift();
            vitalSignsChart.data.datasets[3].data.push(newRR);
            vitalSignsChart.data.datasets[3].data.shift();
            
            // 현재 값 업데이트
            document.getElementById('currentBP').textContent = `${Math.round(newSBP)}/${Math.round(newDBP)}`;
            document.getElementById('currentHR').textContent = Math.round(newHR);
            document.getElementById('currentRR').textContent = Math.round(newRR);
            document.getElementById('currentTemp').textContent = newTemp.toFixed(1);
            
            // TICU 특화 알림 체크
            if (newSBP < 90) {
                document.getElementById('currentBP').style.color = '#f44336';
            } else if (newSBP > 140) {
                document.getElementById('currentBP').style.color = '#ff9800';
            } else {
                document.getElementById('currentBP').style.color = '#667eea';
            }
            
            if (newHR > 100 || newHR < 60) {
                document.getElementById('currentHR').style.color = '#f44336';
            } else {
                document.getElementById('currentHR').style.color = '#667eea';
            }
            
            vitalSignsChart.update('none');
        }, 5000);
        
        // 환자별 AKI 데이터
        const patientAKIData = {
            bed1: {
                name: 'BED1 박OO (65세/남)',
                iss: 28,
                admissionHours: 72,
                diagnosis: '다발성 외상',
                riskScore: 72,
                crChange: '↑ 45%',
                crChangeValue: 45,
                urineOutput: 28,
                riskFactors: [
                    '대량수혈: pRBC 14 units (4hr)',
                    '횡문근융해증: CK 8,500',
                    '저혈압: MAP < 65 (2hr)',
                    '조영제 사용: 복부 CT (3회)'
                ]
            },
            bed5: {
                name: 'BED5 이OO (72세/여)',
                iss: 22,
                admissionHours: 120,
                diagnosis: '복부 관통상',
                riskScore: 45,
                crChange: '↑ 22%',
                crChangeValue: 22,
                urineOutput: 42,
                riskFactors: [
                    '대량수혈: pRBC 8 units (4hr)',
                    '패혈증 위험: WBC 18,000',
                    '승압제 사용: Norepinephrine',
                    '연령: 고령 (72세)'
                ]
            },
            bed8: {
                name: 'BED8 김OO (45세/남)',
                iss: 18,
                admissionHours: 48,
                diagnosis: '흉부 외상',
                riskScore: 25,
                crChange: '↑ 10%',
                crChangeValue: 10,
                urineOutput: 65,
                riskFactors: [
                    '흉부 외상: 늑골 골절',
                    '경미한 수혈: pRBC 4 units',
                    '안정적 활력징후',
                    '정상 요량 유지'
                ]
            }
        };
        
        // AKI 환자 선택 이벤트
        document.getElementById('akiPatientSelect').addEventListener('change', function(e) {
            const selectedBed = e.target.value;
            updateAKICard(selectedBed);
        });
        
        // AKI 카드 업데이트 함수
        function updateAKICard(bedId) {
            const patient = patientAKIData[bedId];
            
            // 환자 정보 업데이트
            document.getElementById('akiPatientInfo').innerHTML = `
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 0.5rem;">
                    <span><strong>${patient.name.split(' ')[0]}</strong> ${patient.name.split(' ')[1]}</span>
                    <span>ISS: ${patient.iss}점</span>
                    <span>입원: ${patient.admissionHours}시간 경과</span>
                    <span>진단: ${patient.diagnosis}</span>
                </div>
            `;
            
            // 위험도 업데이트
            document.getElementById('akiRiskScore').textContent = patient.riskScore + '%';
            const color = patient.riskScore > 70 ? '#f44336' : patient.riskScore > 40 ? '#ff9800' : '#4caf50';
            document.getElementById('akiRiskScore').style.color = color;
            
            // 프로그레스 바 업데이트
            document.getElementById('akiRiskIndicator').style.left = patient.riskScore + '%';
            document.getElementById('akiRiskIndicator').style.background = color;
            
            // 지표 업데이트
            document.getElementById('crChange').textContent = patient.crChange;
            document.getElementById('crChange').style.color = patient.crChangeValue > 30 ? '#f44336' : '#ff9800';
            
            document.getElementById('urineOutput').textContent = patient.urineOutput;
            document.getElementById('urineOutput').style.color = patient.urineOutput < 30 ? '#f44336' : patient.urineOutput < 50 ? '#ff9800' : '#4caf50';
            
            // 위험 요인 업데이트
            const riskFactorsList = patient.riskFactors.map(factor => `<li>${factor}</li>`).join('');
            document.getElementById('akiRiskFactors').innerHTML = `
                <div style="font-weight: 500; margin-bottom: 0.5rem; color: #7b1fa2;">
                    <i class="fas fa-exclamation-circle"></i> 주요 위험 요인
                </div>
                <ul style="margin: 0; padding-left: 1.5rem; font-size: 0.9rem;">
                    ${riskFactorsList}
                </ul>
            `;
        }
        
        // AKI 위험도 실시간 업데이트
        function updateAKIRisk() {
            const selectedBed = document.getElementById('akiPatientSelect').value;
            const patient = patientAKIData[selectedBed];
            
            // 환자별 위험도 변동 시뮬레이션
            const variation = Math.random() * 10 - 5; // -5 to +5 변동
            let newRiskScore = patient.riskScore + variation;
            newRiskScore = Math.max(10, Math.min(95, newRiskScore)); // 10-95% 범위 제한
            
            // 위험도 표시 업데이트
            document.getElementById('akiRiskScore').textContent = Math.round(newRiskScore) + '%';
            
            // 위험도 색상 변경
            const color = newRiskScore > 70 ? '#f44336' : newRiskScore > 40 ? '#ff9800' : '#4caf50';
            document.getElementById('akiRiskScore').style.color = color;
            
            // 프로그레스 바 업데이트
            document.getElementById('akiRiskIndicator').style.left = newRiskScore + '%';
            document.getElementById('akiRiskIndicator').style.background = color;
            
            // Cr 변화율도 약간 변동
            const crVariation = Math.random() * 5 - 2.5;
            const newCrChange = Math.max(0, patient.crChangeValue + crVariation);
            document.getElementById('crChange').textContent = '↑ ' + Math.round(newCrChange) + '%';
            
            // 요량도 변동
            const urineVariation = Math.random() * 10 - 5;
            const newUrineOutput = Math.max(10, patient.urineOutput + urineVariation);
            document.getElementById('urineOutput').textContent = Math.round(newUrineOutput);
        }
        
        // 30초마다 AKI 위험도 업데이트
        setInterval(updateAKIRisk, 30000);
        
        // 예방 프로토콜 버튼 클릭 이벤트
        const preventionBtn = document.querySelector('.fa-clipboard-check').parentElement;
        if (preventionBtn) {
            preventionBtn.addEventListener('click', function() {
                alert('AKI 예방 프로토콜:\n1. 수액 균형 최적화 (CVP 8-12)\n2. 신독성 약물 회피\n3. MAP > 65 유지\n4. 조영제 최소화\n5. 2시간마다 Cr/요량 모니터링');
            });
        }
        
        const analysisBtn = document.querySelector('.fa-chart-bar').parentElement;
        if (analysisBtn) {
            analysisBtn.addEventListener('click', function() {
                alert('상세 분석 페이지로 이동합니다.\n- RIFLE/AKIN/KDIGO criteria 분석\n- 시간대별 위험도 추이\n- 예측 모델 상세 설명');
            });
        }

        // 환자 선택 변경 이벤트
        document.getElementById('patientSelect').addEventListener('change', function(e) {
            const selectedPatient = e.target.value;
            // 환자별로 다른 바이탈 범위 설정
            console.log(`환자 ${selectedPatient}로 전환`);
            // 실제 구현시에는 환자별 데이터를 서버에서 가져와서 차트 업데이트
            
            // AKI 선택도 동기화
            document.getElementById('akiPatientSelect').value = selectedPatient;
            updateAKICard(selectedPatient);
        });
        
        // AKI 환자 선택 이벤트
        document.getElementById('akiPatientSelect').addEventListener('change', function(e) {
            const selectedBed = e.target.value;
            updateAKICard(selectedBed);
        });
        
        // 클릭 이벤트 예시
        document.querySelectorAll('.priority-item').forEach(item => {
            item.addEventListener('click', function() {
                alert('간호 업무 상세 페이지로 이동합니다.');
            });
        });
        
        // 플로팅 버튼 클릭
        document.querySelector('.fab').addEventListener('click', function() {
            alert('새 환자 등록 또는 긴급 알림 생성');
        });
        
        // 초기 AKI 카드 설정
        updateAKICard('bed1');
        
        }); // DOMContentLoaded 종료
    </script>
</body>
</html>
