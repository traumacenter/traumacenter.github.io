<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Temporal Topic Analysis - Trauma Nursing Research</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chartjs-adapter-date-fns/dist/chartjs-adapter-date-fns.bundle.min.js"></script>
    <style>
        body {
            font-family: 'Times New Roman', serif;
            margin: 20px;
            background-color: #f8f9fa;
        }
        .container {
            max-width: 1400px;
            margin: 0 auto;
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 40px;
            font-size: 32px;
            border-bottom: 3px solid #3498db;
            padding-bottom: 15px;
        }
        h2 {
            color: #34495e;
            border-left: 4px solid #3498db;
            padding-left: 15px;
            margin-top: 40px;
            font-size: 24px;
        }
        .analysis-summary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 25px;
            border-radius: 15px;
            margin: 20px 0;
            text-align: center;
        }
        .summary-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        .summary-item {
            background: rgba(255,255,255,0.2);
            padding: 15px;
            border-radius: 10px;
        }
        .summary-value {
            font-size: 2em;
            font-weight: bold;
            display: block;
        }
        .summary-label {
            font-size: 0.9em;
            opacity: 0.9;
        }
        .chart-container {
            margin: 30px 0;
            padding: 25px;
            border: 2px solid #ecf0f1;
            border-radius: 10px;
            background-color: #fdfdfd;
        }
        .chart-title {
            text-align: center;
            font-weight: bold;
            margin-bottom: 25px;
            color: #2c3e50;
            font-size: 18px;
        }
        .trend-analysis {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }
        .trend-card {
            border: 1px solid #ddd;
            border-radius: 8px;
            padding: 20px;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
        }
        .trend-title {
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 15px;
            font-size: 16px;
        }
        .trend-stats {
            font-size: 14px;
            color: #34495e;
        }
        .trend-up {
            color: #27ae60;
            font-weight: bold;
        }
        .trend-down {
            color: #e74c3c;
            font-weight: bold;
        }
        .trend-stable {
            color: #f39c12;
            font-weight: bold;
        }
        .insights-box {
            background-color: #ecf0f1;
            border-left: 5px solid #3498db;
            padding: 20px;
            margin: 20px 0;
            border-radius: 5px;
        }
        .insights-title {
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 10px;
        }
        .heatmap-container {
            overflow-x: auto;
            margin: 20px 0;
        }
        .heatmap-table {
            width: 100%;
            border-collapse: collapse;
            font-size: 12px;
        }
        .heatmap-table th, .heatmap-table td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
        }
        .heatmap-table th {
            background-color: #34495e;
            color: white;
            font-weight: bold;
        }
        .heat-very-high { background-color: #8B0000; color: white; }
        .heat-high { background-color: #DC143C; color: white; }
        .heat-medium { background-color: #FF6347; color: white; }
        .heat-low { background-color: #FFB6C1; color: black; }
        .heat-very-low { background-color: #F0F8FF; color: black; }
        .controls {
            text-align: center;
            margin: 20px 0;
        }
        .control-button {
            background: #3498db;
            color: white;
            border: none;
            padding: 10px 20px;
            margin: 5px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
        }
        .control-button:hover {
            background: #2980b9;
        }
        .control-button.active {
            background: #e74c3c;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Temporal Analysis of Topics in Trauma Nursing Research</h1>
        
        <!-- 분석 요약 -->
        <div class="analysis-summary">
            <h3>Analysis Overview</h3>
            <div class="summary-grid">
                <div class="summary-item">
                    <span class="summary-value">10</span>
                    <span class="summary-label">Years Analyzed</span>
                </div>
                <div class="summary-item">
                    <span class="summary-value">2015-2024</span>
                    <span class="summary-label">Year Range</span>
                </div>
                <div class="summary-item">
                    <span class="summary-value">423</span>
                    <span class="summary-label">Total Documents</span>
                </div>
                <div class="summary-item">
                    <span class="summary-value">10</span>
                    <span class="summary-label">Topics Identified</span>
                </div>
            </div>
        </div>

        <!-- 연도별 토픽 분포 추세 -->
        <h2>1. Topic Distribution Trends Over Time</h2>
        <div class="chart-container">
            <div class="chart-title">Topic Proportions by Year (Line Chart)</div>
            <canvas id="topicTrendsChart" width="1200" height="600"></canvas>
        </div>

        <!-- 연도별 문서 수 -->
        <h2>2. Publication Volume Over Time</h2>
        <div class="chart-container">
            <div class="chart-title">Number of Documents by Year</div>
            <canvas id="documentVolumeChart" width="1200" height="400"></canvas>
        </div>

        <!-- 토픽별 변화율 분석 -->
        <h2>3. Topic Change Rate Analysis</h2>
        <div class="trend-analysis" id="trendAnalysis">
            <!-- 동적으로 생성됨 -->
        </div>

        <!-- 히트맵 -->
        <h2>4. Topic-Year Heatmap</h2>
        <div class="chart-container">
            <div class="chart-title">Topic Intensity by Year</div>
            <div class="heatmap-container" id="heatmapContainer">
                <!-- 동적으로 생성됨 -->
            </div>
        </div>

        <!-- 상관관계 분석 -->
        <h2>5. Topic Correlation Matrix</h2>
        <div class="chart-container">
            <div class="chart-title">Correlation Between Topics Over Time</div>
            <canvas id="correlationChart" width="800" height="800"></canvas>
        </div>

        <!-- 인사이트 -->
        <div class="insights-box">
            <div class="insights-title">Key Insights:</div>
            <div id="insights">
                <!-- 동적으로 생성됨 -->
            </div>
        </div>

        <!-- 상세 주제 정보 -->
        <h2>6. Topic Details and Evolution</h2>
        <div class="chart-container">
            <div class="controls">
                <button class="control-button active" onclick="showAllTopics()">All Topics</button>
                <button class="control-button" onclick="showTopTrending()">Top Trending</button>
                <button class="control-button" onclick="showMostStable()">Most Stable</button>
            </div>
            <canvas id="detailedTrendsChart" width="1200" height="500"></canvas>
        </div>
    </div>

    <script>
        // 데이터 설정
        const yearlyData = {"years": [2015, 2016, 2017, 2018, 2019, 2020, 2021, 2022, 2023, 2024], "topics": [{"values": [15.34279740318813, 11.815023733006754, 5.099622700260141, 11.757363342549302, 11.7096062007263, 11.710442969266968, 11.210636403834165, 8.769304710691951, 9.459589450377571, 8.35766873312337]}, {"values": [5.250336532662052, 9.001173841485638, 9.660357799509258, 7.8381167083627385, 10.337682422297023, 9.477407785371177, 12.091985857847718, 14.812367331637422, 11.569021167651119, 10.645176277640752]}, {"values": [15.84391670859159, 4.417940381941795, 11.606089856879231, 10.771598812751753, 13.118141917116446, 16.33733144566577, 15.39218457416777, 14.905129524266941, 8.400592576229664, 9.205804695354843]}, {"values": [4.840187895250541, 11.077022458811829, 5.628622782259396, 8.870964107511739, 9.724122102689307, 9.126006903218551, 11.988507689117588, 9.41522423883003, 10.140918123705314, 8.208811717064787]}, {"values": [6.0477934692856605, 8.660312442086123, 7.182326678380317, 8.122636869118171, 4.818367740440251, 7.685858636199668, 1.7632634852733753, 6.56489323961663, 3.1546213717013263, 4.181727033947381]}, {"values": [4.974728938761797, 7.390885572341841, 9.35020835398445, 7.3148687732842905, 7.404851757039807, 5.645875592109072, 8.839305771264529, 3.1248237848016767, 12.158488927145914, 9.999697173917742]}, {"values": [13.408361092089946, 11.16945153795648, 18.264386956843598, 13.21355912853091, 15.039405495269914, 11.42335690868556, 11.804754128938718, 15.979484964890577, 16.299318003823206, 16.070159286357804]}, {"values": [7.409390969430945, 6.10126124542953, 8.270568693874452, 11.405699258127118, 6.285963942582851, 9.421272896213486, 8.27110818710986, 8.355242437135345, 10.521670638717733, 10.455575946101572]}, {"values": [17.794178217537947, 22.882431036133468, 18.23572301528636, 16.1016708386662, 16.72792101079829, 10.496493856101218, 11.088568447976682, 15.189257922809855, 15.104390316928177, 15.450000791405449]}, {"values": [9.08830877320139, 7.484497750806543, 6.7020931627228, 4.603522161097774, 4.833937411039808, 8.67595300716853, 7.5496854544695955, 2.8842718453195766, 3.1913894237199676, 7.425378345086305]}]};
        const topicNames = ["improvement + performance + time", "stress + disorder + burnout", "mortality + iss + admission", "pediatric + child + prevention", "pain + violence + safety", "blood + transfusion + time", "level + from + center", "screen + discharge + intervention", "education + training + team", "fracture + vehicle + txa"];
        const yearlyStats = {"document_counts": [34, 39, 42, 47, 40, 48, 45, 43, 44, 41]};
        const correlationData = [[1.0, -0.5566798002081472, 0.23969260453959523, 0.06535779887788842, 0.06280431268417236, -0.41045475070395876, -0.7421511553855706, -0.2556803800633391, 0.0006508524039378462, 0.355780645042817], [-0.5566798002081472, 1.0, 0.04307141803290648, 0.5633496878539218, -0.396751248227261, 0.05612609863105039, 0.3102931608111016, 0.07975303798336118, -0.3576598544366106, -0.5985118854330863], [0.23969260453959523, 0.04307141803290648, 1.0, -0.2512553132577901, -0.18281173717967666, -0.5409499056344873, -0.08678082656952954, -0.030448799208388753, -0.6800227124242101, 0.19050389834980755], [0.06535779887788842, 0.5633496878539218, -0.2512553132577901, 1.0, -0.30740727658293177, 0.17900611403057107, -0.4503281587569955, -0.038240448843678165, -0.23465958945738513, -0.29821434322265716], [0.06280431268417236, -0.396751248227261, -0.18281173717967666, -0.30740727658293177, 1.0, -0.509020892767659, -0.18529544120987063, -0.12299112737907512, 0.4820768256853226, 0.11844145848625726], [-0.41045475070395876, 0.05612609863105039, -0.5409499056344873, 0.17900611403057107, -0.509020892767659, 1.0, 0.32389485695461023, 0.35416046404923807, 0.006052546446221611, -0.10245024205717637], [-0.7421511553855706, 0.3102931608111016, -0.08678082656952954, -0.4503281587569955, -0.18529544120987063, 0.32389485695461023, 1.0, 0.20546876033295697, 0.1313020113616996, -0.4939204526486581], [-0.2556803800633391, 0.07975303798336118, -0.030448799208388753, -0.038240448843678165, -0.12299112737907512, 0.35416046404923807, 0.20546876033295697, 1.0, -0.47144834659875606, -0.25905832870942064], [0.0006508524039378462, -0.3576598544366106, -0.6800227124242101, -0.23465958945738513, 0.4820768256853226, 0.006052546446221611, 0.1313020113616996, -0.47144834659875606, 1.0, -0.021559186359233887], [0.355780645042817, -0.5985118854330863, 0.19050389834980755, -0.29821434322265716, 0.11844145848625726, -0.10245024205717637, -0.4939204526486581, -0.25905832870942064, -0.021559186359233887, 1.0]];
        const trendAnalysis = [{"change_rate": -0.03607906966742481, "peak_year": 2015, "avg_proportion": 10.523205564702465}, {"change_rate": 0.06271662842495568, "peak_year": 2022, "avg_proportion": 10.06836257244649}, {"change_rate": 0.0008645053026752979, "peak_year": 2020, "avg_proportion": 11.99987304929658}, {"change_rate": 0.03502936528401299, "peak_year": 2021, "avg_proportion": 8.902038801845908}, {"change_rate": -0.07774193119137443, "peak_year": 2016, "avg_proportion": 5.81818009660489}, {"change_rate": 0.03999275046877146, "peak_year": 2023, "avg_proportion": 7.620373464465112}, {"change_rate": 0.017245832730671415, "peak_year": 2017, "avg_proportion": 14.267223750338673}, {"change_rate": 0.03679439371729778, "peak_year": 2018, "avg_proportion": 8.649775421472288}, {"change_rate": -0.042689989371888636, "peak_year": 2016, "avg_proportion": 15.907063545364366}, {"change_rate": -0.04991703660683065, "peak_year": 2015, "avg_proportion": 6.243903733463229}];

        // 색상 팔레트
        const colors = [
            '#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF',
            '#FF9F40', '#FF6384', '#C9CBCF', '#4BC0C0', '#FF6384'
        ];

        // 1. 토픽 추세 차트
        const ctx1 = document.getElementById('topicTrendsChart').getContext('2d');
        const trendsChart = new Chart(ctx1, {
            type: 'line',
            data: {
                labels: yearlyData.years,
                datasets: yearlyData.topics.map((topic, index) => ({
                    label: `Topic ${index + 1}: ${topicNames[index]}`,
                    data: topic.values,
                    borderColor: colors[index % colors.length],
                    backgroundColor: colors[index % colors.length] + '20',
                    fill: false,
                    tension: 0.4,
                    pointRadius: 4,
                    pointHoverRadius: 6
                }))
            },
            options: {
                responsive: true,
                interaction: {
                    intersect: false,
                    mode: 'index'
                },
                scales: {
                    x: {
                        title: {
                            display: true,
                            text: 'Year',
                            font: {
                                size: 14,
                                weight: 'bold'
                            }
                        }
                    },
                    y: {
                        title: {
                            display: true,
                            text: 'Topic Proportion (%)',
                            font: {
                                size: 14,
                                weight: 'bold'
                            }
                        },
                        beginAtZero: true
                    }
                },
                plugins: {
                    legend: {
                        position: 'right',
                        labels: {
                            font: {
                                size: 10
                            },
                            boxWidth: 12
                        }
                    },
                    tooltip: {
                        callbacks: {
                            title: function(context) {
                                return 'Year: ' + context[0].label;
                            },
                            label: function(context) {
                                return context.dataset.label + ': ' + context.parsed.y.toFixed(1) + '%';
                            }
                        }
                    }
                }
            }
        });

        // 2. 문서 볼륨 차트
        const ctx2 = document.getElementById('documentVolumeChart').getContext('2d');
        new Chart(ctx2, {
            type: 'bar',
            data: {
                labels: yearlyData.years,
                datasets: [{
                    label: 'Number of Documents',
                    data: yearlyStats.document_counts,
                    backgroundColor: 'rgba(52, 152, 219, 0.8)',
                    borderColor: 'rgba(52, 152, 219, 1)',
                    borderWidth: 2
                }]
            },
            options: {
                responsive: true,
                scales: {
                    x: {
                        title: {
                            display: true,
                            text: 'Year',
                            font: {
                                size: 14,
                                weight: 'bold'
                            }
                        }
                    },
                    y: {
                        title: {
                            display: true,
                            text: 'Document Count',
                            font: {
                                size: 14,
                                weight: 'bold'
                            }
                        },
                        beginAtZero: true
                    }
                },
                plugins: {
                    legend: {
                        display: false
                    }
                }
            }
        });

        // 3. 추세 분석 카드 생성
        function createTrendAnalysis() {
            const container = document.getElementById('trendAnalysis');
            
            trendAnalysis.forEach((trend, index) => {
                const card = document.createElement('div');
                card.className = 'trend-card';
                
                let trendClass = 'trend-stable';
                let trendText = 'Stable';
                if (trend.change_rate > 0.1) {
                    trendClass = 'trend-up';
                    trendText = 'Increasing';
                } else if (trend.change_rate < -0.1) {
                    trendClass = 'trend-down';
                    trendText = 'Decreasing';
                }
                
                card.innerHTML = `
                    <div class="trend-title">Topic ${index + 1}: ${topicNames[index]}</div>
                    <div class="trend-stats">
                        <div>Trend: <span class="${trendClass}">${trendText}</span></div>
                        <div>Change Rate: <span class="${trendClass}">${(trend.change_rate * 100).toFixed(1)}%</span></div>
                        <div>Peak Year: ${trend.peak_year}</div>
                        <div>Avg Proportion: ${trend.avg_proportion.toFixed(1)}%</div>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        // 4. 히트맵 생성
        function createHeatmap() {
            const container = document.getElementById('heatmapContainer');
            
            let html = '<table class="heatmap-table"><thead><tr><th>Topic</th>';
            yearlyData.years.forEach(year => {
                html += `<th>${year}</th>`;
            });
            html += '</tr></thead><tbody>';

            yearlyData.topics.forEach((topic, topicIndex) => {
                html += `<tr><td><strong>Topic ${topicIndex + 1}</strong></td>`;
                topic.values.forEach(value => {
                    let heatClass = 'heat-very-low';
                    if (value > 20) heatClass = 'heat-very-high';
                    else if (value > 15) heatClass = 'heat-high';
                    else if (value > 10) heatClass = 'heat-medium';
                    else if (value > 5) heatClass = 'heat-low';
                    
                    html += `<td class="${heatClass}">${value.toFixed(1)}%</td>`;
                });
                html += '</tr>';
            });
            
            html += '</tbody></table>';
            container.innerHTML = html;
        }

        // 5. 상관관계 매트릭스
        const ctx5 = document.getElementById('correlationChart').getContext('2d');
        new Chart(ctx5, {
            type: 'scatter',
            data: {
                datasets: correlationData.map((corr, index) => ({
                    label: `Topic ${index + 1} Correlations`,
                    data: corr.map((value, i) => ({x: i, y: index, v: value})),
                    backgroundColor: function(context) {
                        const value = context.parsed.v;
                        const alpha = Math.abs(value);
                        return value > 0 ? `rgba(231, 76, 60, ${alpha})` : `rgba(52, 152, 219, ${alpha})`;
                    },
                    pointRadius: 15
                }))
            },
            options: {
                responsive: true,
                scales: {
                    x: {
                        type: 'linear',
                        position: 'bottom',
                        min: 0,
                        max: 10 - 1,
                        title: {
                            display: true,
                            text: 'Topic Index'
                        }
                    },
                    y: {
                        type: 'linear',
                        min: 0,
                        max: 10 - 1,
                        title: {
                            display: true,
                            text: 'Topic Index'
                        }
                    }
                },
                plugins: {
                    legend: {
                        display: false
                    },
                    tooltip: {
                        callbacks: {
                            label: function(context) {
                                return `Correlation: ${context.parsed.v.toFixed(3)}`;
                            }
                        }
                    }
                }
            }
        });

        // 6. 상세 추세 차트 (제어 가능)
        let detailedChart;
        
        function createDetailedChart(selectedTopics) {
            if (detailedChart) {
                detailedChart.destroy();
            }
            
            const ctx6 = document.getElementById('detailedTrendsChart').getContext('2d');
            detailedChart = new Chart(ctx6, {
                type: 'line',
                data: {
                    labels: yearlyData.years,
                    datasets: selectedTopics.map(index => ({
                        label: `Topic ${index + 1}: ${topicNames[index]}`,
                        data: yearlyData.topics[index].values,
                        borderColor: colors[index % colors.length],
                        backgroundColor: colors[index % colors.length] + '30',
                        fill: true,
                        tension: 0.4,
                        pointRadius: 5,
                        pointHoverRadius: 8,
                        borderWidth: 3
                    }))
                },
                options: {
                    responsive: true,
                    interaction: {
                        intersect: false,
                        mode: 'index'
                    },
                    scales: {
                        x: {
                            title: {
                                display: true,
                                text: 'Year',
                                font: {
                                    size: 14,
                                    weight: 'bold'
                                }
                            }
                        },
                        y: {
                            title: {
                                display: true,
                                text: 'Topic Proportion (%)',
                                font: {
                                    size: 14,
                                    weight: 'bold'
                                }
                            },
                            beginAtZero: true
                        }
                    },
                    plugins: {
                        legend: {
                            position: 'top',
                            labels: {
                                font: {
                                    size: 12
                                }
                            }
                        }
                    }
                }
            });
        }

        // 컨트롤 함수들
        function showAllTopics() {
            document.querySelectorAll('.control-button').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            createDetailedChart([0, 1, 2, 3, 4, 5, 6, 7, 8, 9]);
        }

        function showTopTrending() {
            document.querySelectorAll('.control-button').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            
            // 변화율이 높은 상위 5개 토픽
            const topTrending = trendAnalysis
                .map((trend, index) => ({index, change_rate: trend.change_rate}))
                .sort((a, b) => Math.abs(b.change_rate) - Math.abs(a.change_rate))
                .slice(0, 5)
                .map(item => item.index);
            
            createDetailedChart(topTrending);
        }

        function showMostStable() {
            document.querySelectorAll('.control-button').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            
            // 변화율이 낮은 상위 5개 토픽
            const mostStable = trendAnalysis
                .map((trend, index) => ({index, change_rate: Math.abs(trend.change_rate)}))
                .sort((a, b) => a.change_rate - b.change_rate)
                .slice(0, 5)
                .map(item => item.index);
            
            createDetailedChart(mostStable);
        }

        // 인사이트 생성
        function generateInsights() {
            const insights = [];
            
            // 가장 급성장하는 토픽
            const fastestGrowing = trendAnalysis.reduce((max, trend, index) => 
                trend.change_rate > max.change_rate ? {...trend, index} : max
            );
            insights.push(`<strong>Fastest Growing Topic:</strong> Topic ${fastestGrowing.index + 1} (${topicNames[fastestGrowing.index]}) with ${(fastestGrowing.change_rate * 100).toFixed(1)}% growth rate.`);
            
            // 가장 감소하는 토픽
            const fastestDeclining = trendAnalysis.reduce((min, trend, index) => 
                trend.change_rate < min.change_rate ? {...trend, index} : min
            );
            insights.push(`<strong>Fastest Declining Topic:</strong> Topic ${fastestDeclining.index + 1} (${topicNames[fastestDeclining.index]}) with ${(fastestDeclining.change_rate * 100).toFixed(1)}% decline rate.`);
            
            // 가장 안정적인 토픽
            const mostStable = trendAnalysis.reduce((min, trend, index) => 
                Math.abs(trend.change_rate) < Math.abs(min.change_rate) ? {...trend, index} : min
            );
            insights.push(`<strong>Most Stable Topic:</strong> Topic ${mostStable.index + 1} (${topicNames[mostStable.index]}) with minimal change (${(mostStable.change_rate * 100).toFixed(1)}%).`);
            
            // 연구 활동이 가장 활발한 연도
            const maxDocsIndex = yearlyStats.document_counts.indexOf(Math.max(...yearlyStats.document_counts));
            const peakYear = yearlyData.years[maxDocsIndex];
            insights.push(`<strong>Peak Research Year:</strong> ${peakYear} with ${yearlyStats.document_counts[maxDocsIndex]} documents published.`);
            
            document.getElementById('insights').innerHTML = insights.join('<br><br>');
        }

        // 초기화
        createTrendAnalysis();
        createHeatmap();
        createDetailedChart([0, 1, 2, 3, 4, 5, 6, 7, 8, 9]);
        generateInsights();
    </script>
</body>
</html>
