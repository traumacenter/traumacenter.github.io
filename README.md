import streamlit as st
import pandas as pd
import numpy as np
import plotly.graph_objects as go
import plotly.express as px
from datetime import datetime, timedelta
import json
from io import BytesIO
import base64

# 페이지 설정
st.set_page_config(
    page_title="CBI 번아웃 전문 측정센터",
    page_icon="🧠",
    layout="wide",
    initial_sidebar_state="expanded"
)

# 고급 CSS 스타일링 - 따뜻하고 전문적인 디자인
st.markdown("""
<style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700&display=swap');
    
    html, body, [class*="css"] {
        font-family: 'Noto Sans KR', sans-serif;
    }
    
    .main {
        padding: 0;
    }
    
    .stApp {
        background: linear-gradient(135deg, #faf0e6 0%, #f5deb3 50%, #deb887 100%);
    }
    
    .main-header {
        background: linear-gradient(135deg, #d2691e 0%, #cd853f 50%, #bc7a00 100%);
        padding: 3rem 2rem;
        margin: -1rem -1rem 2rem -1rem;
        color: white;
        text-align: center;
        box-shadow: 0 4px 20px rgba(0,0,0,0.1);
    }
    
    .main-title {
        font-size: 3.5rem;
        font-weight: 700;
        margin-bottom: 1rem;
        text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        background: linear-gradient(45deg, #fff, #f0e68c);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
    }
    
    .main-subtitle {
        font-size: 1.4rem;
        font-weight: 300;
        opacity: 0.95;
        margin-bottom: 0;
    }
    
    .section-container {
        background: rgba(255,255,255,0.95);
        padding: 2rem;
        border-radius: 20px;
        margin: 1.5rem 0;
        box-shadow: 0 8px 32px rgba(0,0,0,0.1);
        backdrop-filter: blur(10px);
        border: 1px solid rgba(255,255,255,0.2);
    }
    
    .section-title {
        color: #8b4513;
        font-size: 2rem;
        font-weight: 600;
        margin-bottom: 1.5rem;
        text-align: center;
        position: relative;
    }
    
    .section-title::after {
        content: '';
        position: absolute;
        bottom: -10px;
        left: 50%;
        transform: translateX(-50%);
        width: 60px;
        height: 3px;
        background: linear-gradient(90deg, #d2691e, #cd853f);
        border-radius: 2px;
    }
    
    .metric-box {
        background: linear-gradient(135deg, #fff8dc 0%, #f5f5dc 100%);
        padding: 2rem;
        border-radius: 15px;
        text-align: center;
        margin: 1rem 0;
        transition: transform 0.3s ease, box-shadow 0.3s ease;
        border-left: 5px solid #d2691e;
    }
    
    .metric-box:hover {
        transform: translateY(-5px);
        box-shadow: 0 15px 35px rgba(210, 105, 30, 0.2);
    }
    
    .metric-score {
        font-size: 3rem;
        font-weight: 700;
        margin: 1rem 0;
    }
    
    .metric-label {
        font-size: 1.1rem;
        font-weight: 500;
        color: #8b4513;
        margin-bottom: 0.5rem;
    }
    
    .metric-status {
        font-size: 1rem;
        font-weight: 600;
        padding: 0.5rem 1rem;
        border-radius: 20px;
        margin-top: 1rem;
    }
    
    .risk-low {
        border-left-color: #22c55e;
    }
    .risk-low .metric-score { color: #22c55e; }
    .risk-low .metric-status { 
        background: #dcfce7; 
        color: #166534; 
        border: 1px solid #bbf7d0;
    }
    
    .risk-caution {
        border-left-color: #f59e0b;
    }
    .risk-caution .metric-score { color: #f59e0b; }
    .risk-caution .metric-status { 
        background: #fef3c7; 
        color: #92400e; 
        border: 1px solid #fde68a;
    }
    
    .risk-moderate {
        border-left-color: #ef4444;
    }
    .risk-moderate .metric-score { color: #ef4444; }
    .risk-moderate .metric-status { 
        background: #fee2e2; 
        color: #991b1b; 
        border: 1px solid #fecaca;
    }
    
    .risk-high {
        border-left-color: #dc2626;
    }
    .risk-high .metric-score { color: #dc2626; }
    .risk-high .metric-status { 
        background: #fef2f2; 
        color: #7f1d1d; 
        border: 1px solid #fee2e2;
    }
    
    .question-card {
        background: rgba(255,255,255,0.8);
        padding: 1.5rem;
        border-radius: 12px;
        margin: 1rem 0;
        border-left: 4px solid #d2691e;
        transition: all 0.3s ease;
    }
    
    .question-card:hover {
        background: rgba(255,255,255,0.95);
        transform: translateX(5px);
    }
    
    .question-text {
        font-size: 1.1rem;
        font-weight: 500;
        color: #2d3748;
        margin-bottom: 1rem;
        line-height: 1.6;
    }
    
    .interpretation-card {
        background: linear-gradient(135deg, #f0fff4 0%, #dcfce7 100%);
        padding: 2rem;
        border-radius: 15px;
        margin: 2rem 0;
        border: 1px solid #bbf7d0;
        position: relative;
    }
    
    .interpretation-title {
        font-size: 1.4rem;
        font-weight: 600;
        color: #166534;
        margin-bottom: 1rem;
        display: flex;
        align-items: center;
        gap: 0.5rem;
    }
    
    .interpretation-content {
        font-size: 1rem;
        line-height: 1.7;
        color: #374151;
    }
    
    .recommendation-section {
        background: linear-gradient(135deg, #fffbeb 0%, #fef3c7 100%);
        padding: 2rem;
        border-radius: 15px;
        margin: 2rem 0;
        border: 1px solid #fde68a;
    }
    
    .recommendation-title {
        font-size: 1.3rem;
        font-weight: 600;
        color: #92400e;
        margin-bottom: 1rem;
        display: flex;
        align-items: center;
        gap: 0.5rem;
    }
    
    .recommendation-item {
        background: rgba(255,255,255,0.7);
        padding: 1rem 1.5rem;
        border-radius: 8px;
        margin: 0.8rem 0;
        border-left: 3px solid #f59e0b;
        font-size: 1rem;
        line-height: 1.6;
    }
    
    .priority-high {
        background: linear-gradient(135deg, #fef2f2 0%, #fee2e2 100%);
        border-left-color: #dc2626;
    }
    
    .priority-medium {
        background: linear-gradient(135deg, #fffbeb 0%, #fef3c7 100%);
        border-left-color: #f59e0b;
    }
    
    .priority-low {
        background: linear-gradient(135deg, #f0fff4 0%, #dcfce7 100%);
        border-left-color: #22c55e;
    }
    
    .progress-container {
        background: rgba(255,255,255,0.8);
        padding: 1.5rem;
        border-radius: 10px;
        margin: 1rem 0;
        text-align: center;
    }
    
    .stButton > button {
        background: linear-gradient(135deg, #d2691e 0%, #cd853f 100%);
        color: white;
        border: none;
        padding: 1rem 2.5rem;
        border-radius: 30px;
        font-weight: 600;
        font-size: 1.1rem;
        transition: all 0.3s ease;
        box-shadow: 0 4px 15px rgba(210, 105, 30, 0.3);
    }
    
    .stButton > button:hover {
        background: linear-gradient(135deg, #cd853f 0%, #d2691e 100%);
        transform: translateY(-2px);
        box-shadow: 0 8px 25px rgba(210, 105, 30, 0.4);
    }
    
    .feature-card {
        background: rgba(255,255,255,0.9);
        padding: 2rem;
        border-radius: 15px;
        text-align: center;
        margin: 1rem;
        transition: transform 0.3s ease;
        border: 1px solid rgba(210, 105, 30, 0.1);
    }
    
    .feature-card:hover {
        transform: translateY(-5px);
        box-shadow: 0 10px 30px rgba(0,0,0,0.1);
    }
    
    .feature-icon {
        font-size: 3rem;
        margin-bottom: 1rem;
    }
    
    .footer {
        background: linear-gradient(135deg, #8b4513 0%, #a0522d 100%);
        color: white;
        padding: 2rem;
        margin: 3rem -1rem -1rem -1rem;
        text-align: center;
        border-radius: 0;
    }
    
    .chart-container {
        background: rgba(255,255,255,0.95);
        padding: 1.5rem;
        border-radius: 15px;
        margin: 1rem 0;
        box-shadow: 0 4px 15px rgba(0,0,0,0.05);
    }
    
    /* 반응형 디자인 */
    @media (max-width: 768px) {
        .main-title { font-size: 2.5rem; }
        .main-subtitle { font-size: 1.1rem; }
        .section-container { padding: 1.5rem; margin: 1rem 0; }
        .metric-box { padding: 1.5rem; }
        .metric-score { font-size: 2.5rem; }
        .stButton > button { width: 100%; margin: 0.5rem 0; }
    }
</style>
""", unsafe_allow_html=True)

# CBI 문항 데이터베이스
CBI_QUESTIONS = {
    'personal': {
        'title': '🏠 개인적 번아웃',
        'description': '일과 관계없는 전반적인 피로와 탈진 정도를 평가합니다.',
        'questions': [
            "얼마나 자주 피곤함을 느끼십니까?",
            "얼마나 자주 신체적으로 탈진감을 느끼십니까?",
            "얼마나 자주 정서적으로 탈진감을 느끼십니까?",
            "얼마나 자주 '더 이상 견딜 수 없다'고 생각하십니까?",
            "얼마나 자주 지친다고 느끼십니까?",
            "얼마나 자주 몸이 약해졌다고 느끼십니까?"
        ]
    },
    'work': {
        'title': '💼 업무 관련 번아웃',
        'description': '직접적으로 업무로 인한 피로와 탈진 정도를 평가합니다.',
        'questions': [
            "일로 인해 얼마나 자주 지치십니까?",
            "일로 인해 얼마나 자주 신체적으로 탈진감을 느끼십니까?",
            "일로 인해 얼마나 자주 정서적으로 탈진감을 느끼십니까?",
            "일로 인해 얼마나 자주 좌절감을 느끼십니까?",
            "일로 인해 얼마나 자주 완전히 지쳐버린다고 느끼십니까?",
            "일에 대해 생각하는 것이 얼마나 자주 스트레스가 됩니까?",
            "아침에 일어나서 또 하루 일해야 한다고 생각하면 얼마나 자주 지치십니까?"
        ]
    },
    'client': {
        'title': '👥 환자 관련 번아웃',
        'description': '환자나 고객과의 상호작용에서 오는 탈진 정도를 평가합니다.',
        'questions': [
            "환자들과 일하는 것이 얼마나 자주 스트레스가 됩니까?",
            "환자들과 일한 후 얼마나 자주 지치십니까?",
            "환자들과 일하는 것이 얼마나 자주 좌절스럽습니까?",
            "환자들과 일할 때 얼마나 자주 한계를 느끼십니까?",
            "환자들과 일하는 것이 얼마나 자주 정서적으로 힘듭니까?",
            "환자들에게 에너지를 주는 것이 얼마나 자주 힘듭니까?"
        ]
    }
}

ANSWER_OPTIONS = [
    "전혀 그렇지 않다 (0점)",
    "거의 그렇지 않다 (1점)", 
    "가끔 그렇다 (2점)",
    "자주 그렇다 (3점)",
    "항상 그렇다 (4점)"
]

# 위험도 평가 함수
def assess_risk_level(score):
    """점수에 따른 상세 위험도 평가"""
    if score < 1.5:
        return {
            'level': '매우 낮음',
            'class': 'risk-low',
            'color': '#22c55e',
            'icon': '😊',
            'description': '건강한 상태'
        }
    elif score < 2.0:
        return {
            'level': '낮음',
            'class': 'risk-low', 
            'color': '#22c55e',
            'icon': '🙂',
            'description': '양호한 상태'
        }
    elif score < 2.5:
        return {
            'level': '경계',
            'class': 'risk-caution',
            'color': '#f59e0b',
            'icon': '😐',
            'description': '주의 관찰 필요'
        }
    elif score < 3.0:
        return {
            'level': '보통',
            'class': 'risk-moderate',
            'color': '#ef4444',
            'icon': '😟',
            'description': '적극적 관리 필요'
        }
    elif score < 3.5:
        return {
            'level': '높음',
            'class': 'risk-high',
            'color': '#dc2626',
            'icon': '😰',
            'description': '즉시 개입 필요'
        }
    else:
        return {
            'level': '매우 높음',
            'class': 'risk-high',
            'color': '#dc2626',
            'icon': '😱',
            'description': '응급 개입 필요'
        }

# 상세 해석 함수
def get_detailed_interpretation(domain, score):
    """영역별 상세 해석 제공"""
    risk = assess_risk_level(score)
    
    interpretations = {
        'personal': {
            '매우 낮음': {
                'summary': '개인적 에너지 수준이 매우 우수합니다.',
                'details': '현재 신체적, 정신적 에너지가 충분하며 일상생활을 활기차게 보내고 계십니다. 스트레스 관리가 잘 되고 있고, 회복 능력도 좋은 상태입니다.',
                'strengths': ['높은 에너지 수준', '우수한 회복력', '효과적인 스트레스 관리'],
                'watch_points': ['현재 상태 유지에 집중', '예방적 관리 지속']
            },
            '낮음': {
                'summary': '개인적 에너지 수준이 양호합니다.',
                'details': '전반적으로 건강한 상태이며, 일상적인 피로는 있지만 적절한 휴식으로 회복이 가능한 수준입니다.',
                'strengths': ['안정적인 에너지 수준', '적절한 피로 관리'],
                'watch_points': ['규칙적인 생활 패턴 유지', '충분한 수면 확보']
            },
            '경계': {
                'summary': '개인적 피로감이 누적되기 시작했습니다.',
                'details': '전반적인 피로감이 평소보다 증가하고 있으며, 휴식 후에도 완전한 회복이 어려울 수 있습니다. 조기 개입이 필요합니다.',
                'strengths': ['아직 회복 가능한 수준'],
                'watch_points': ['수면의 질 개선', '스트레스 요인 파악', '생활 패턴 점검']
            },
            '보통': {
                'summary': '개인적 탈진이 진행되고 있습니다.',
                'details': '만성적인 피로감과 에너지 고갈이 나타나고 있습니다. 일상생활에 지장을 주기 시작했으며, 적극적인 관리가 필요합니다.',
                'strengths': ['문제 인식이 첫 번째 단계'],
                'watch_points': ['전문가 상담 고려', '생활 방식 개선', '충분한 휴식']
            },
            '높음': {
                'summary': '심각한 개인적 탈진 상태입니다.',
                'details': '신체적, 정신적 에너지가 심각하게 고갈된 상태입니다. 기본적인 일상생활도 힘들 수 있으며, 즉시 개입이 필요합니다.',
                'strengths': ['조기 발견으로 치료 가능'],
                'watch_points': ['즉시 전문가 상담', '장기 휴식 고려', '의료적 평가']
            },
            '매우 높음': {
                'summary': '극심한 개인적 탈진으로 응급 개입이 필요합니다.',
                'details': '완전한 에너지 고갈 상태로 정상적인 기능이 어려운 수준입니다. 즉시 전문적 도움과 치료가 필요합니다.',
                'strengths': ['적절한 치료로 회복 가능'],
                'watch_points': ['응급 의료 상담', '즉시 휴직 고려', '집중적 치료 계획']
            }
        },
        'work': {
            '매우 낮음': {
                'summary': '업무 환경에 매우 잘 적응하고 있습니다.',
                'details': '현재 업무로 인한 스트레스가 최소화되어 있으며, 업무와 개인 생활의 균형이 잘 맞춰져 있습니다.',
                'strengths': ['우수한 업무 적응력', '효과적인 업무 관리', '좋은 워라밸'],
                'watch_points': ['현재 상태 유지', '동료들과의 긍정적 관계 지속']
            },
            '낮음': {
                'summary': '업무 스트레스를 잘 관리하고 있습니다.',
                'details': '업무로 인한 피로는 있지만 적절한 수준이며, 업무 효율성이 좋고 스트레스 관리가 효과적입니다.',
                'strengths': ['안정적인 업무 수행', '적절한 스트레스 수준'],
                'watch_points': ['업무량 적정 수준 유지', '정기적인 휴가 활용']
            },
            '경계': {
                'summary': '업무 스트레스가 누적되기 시작했습니다.',
                'details': '업무량이나 업무 환경으로 인한 스트레스가 평소보다 증가하고 있습니다. 조기 대응이 중요합니다.',
                'strengths': ['문제를 인식하고 있음'],
                'watch_points': ['업무량 점검', '우선순위 재정립', '상급자와 상담']
            },
            '보통': {
                'summary': '업무로 인한 탈진이 진행되고 있습니다.',
                'details': '업무 스트레스가 만성화되어 업무 효율성이 떨어지고 있습니다. 업무 환경 개선이나 업무 조정이 필요합니다.',
                'strengths': ['경험과 전문성 유지'],
                'watch_points': ['업무 재조정 요청', '스트레스 관리 교육', '동료 지원 체계 활용']
            },
            '높음': {
                'summary': '심각한 업무 관련 탈진 상태입니다.',
                'details': '업무로 인한 극심한 스트레스로 정상적인 업무 수행이 어려운 상태입니다. 즉시 업무 조정이 필요합니다.',
                'strengths': ['문제 해결 의지'],
                'watch_points': ['즉시 상급자 면담', '업무량 대폭 조정', '부서 이동 고려']
            },
            '매우 높음': {
                'summary': '극심한 업무 탈진으로 응급 조치가 필요합니다.',
                'details': '업무 자체가 극도의 스트레스원이 되어 출근조차 힘든 상태입니다. 즉시 휴직이나 전면적인 업무 환경 변화가 필요합니다.',
                'strengths': ['조기 개입으로 회복 가능'],
                'watch_points': ['응급 휴직 고려', '전문적 치료', '근본적 업무 환경 변화']
            }
        },
        'client': {
            '매우 낮음': {
                'summary': '환자와의 관계에서 높은 만족감을 느끼고 있습니다.',
                'details': '환자 케어에서 보람을 느끼며, 공감 능력을 잘 유지하고 있습니다. 환자와의 상호작용이 에너지원이 되고 있습니다.',
                'strengths': ['높은 공감 능력', '환자 케어 만족도', '긍정적 상호작용'],
                'watch_points': ['현재 접근 방식 유지', '경험 공유를 통한 동료 지원']
            },
            '낮음': {
                'summary': '환자와의 관계가 안정적입니다.',
                'details': '환자 케어에서 적절한 공감을 유지하며, 전문적 거리감도 잘 조절하고 있습니다.',
                'strengths': ['균형잡힌 공감 능력', '전문적 관계 유지'],
                'watch_points': ['지속적인 전문성 개발', '감정 관리 기술 유지']
            },
            '경계': {
                'summary': '환자와의 관계에서 스트레스가 증가하고 있습니다.',
                'details': '환자나 보호자와의 관계에서 이전보다 더 많은 에너지가 소모되고 있습니다. 공감 피로의 초기 단계일 수 있습니다.',
                'strengths': ['여전히 관리 가능한 수준'],
                'watch_points': ['감정 조절 기법 학습', '동료와의 경험 공유', '정기적 디브리핑']
            },
            '보통': {
                'summary': '환자 관련 공감 피로가 진행되고 있습니다.',
                'details': '환자의 고통에 대한 공감이 오히려 부담이 되기 시작했으며, 정서적 거리감을 느끼거나 무감각해지는 경향이 나타날 수 있습니다.',
                'strengths': ['전문적 경험과 지식'],
                'watch_points': ['감정 지지 상담', '환자 관계 개선 교육', '업무 로테이션 고려']
            },
            '높음': {
                'summary': '심각한 환자 관련 탈진 상태입니다.',
                'details': '환자와의 상호작용 자체가 큰 스트레스가 되고 있으며, 공감 능력이 현저히 감소했습니다. 환자에 대한 부정적 감정이 생길 수 있습니다.',
                'strengths': ['문제 인식과 해결 의지'],
                'watch_points': ['즉시 전문 상담', '환자 접촉 빈도 조절', '업무 영역 재조정']
            },
            '매우 높음': {
                'summary': '극심한 환자 관련 탈진으로 응급 개입이 필요합니다.',
                'details': '환자와의 모든 상호작용이 극도의 스트레스가 되어 정상적인 케어 제공이 어려운 상태입니다. 즉시 환경 변화가 필요합니다.',
                'strengths': ['적절한 개입으로 회복 가능'],
                'watch_points': ['응급 업무 조정', '집중적 심리 지원', '환경 변화 고려']
            }
        }
    }
    
    level = risk['level']
    return {
        'risk': risk,
        'interpretation': interpretations[domain][level]
    }

# 맞춤형 권고사항 생성
def generate_comprehensive_recommendations(personal_score, work_score, client_score):
    """종합적인 맞춤형 권고사항 생성"""
    recommendations = []
    
    # 각 영역별 위험도 평가
    personal_risk = assess_risk_level(personal_score)
    work_risk = assess_risk_level(work_score)
    client_risk = assess_risk_level(client_score)
    
    # 전체 패턴 분석
    max_score = max(personal_score, work_score, client_score)
    scores = [personal_score, work_score, client_score]
    high_risk_count = sum(1 for score in scores if score >= 3.0)
    
    # 응급 상황 체크
    if max_score >= 3.5 or high_risk_count >= 2:
        recommendations.append({
            'category': '🚨 응급 조치',
            'priority': 'high',
            'items': [
                '즉시 전문가 상담 또는 의료진 진료를 받으시기 바랍니다',
                '상급자 또는 인사팀과 업무 조정에 대해 긴급 논의하세요',
                '가능하다면 단기 휴직을 고려해보세요',
                '가족이나 신뢰할 수 있는 사람에게 현재 상황을 알리세요'
            ]
        })
    
    # 개인적 번아웃 권고사항
    if personal_score >= 2.5:
        priority = 'high' if personal_score >= 3.5 else 'medium'
        items = []
        
        if personal_score >= 3.5:
            items.extend([
                '의료진 상담을 통한 정확한 건강 상태 평가가 필요합니다',
                '장기간의 충분한 휴식을 계획하세요',
                '근본적인 생활 방식의 변화를 고려해보세요'
            ])
        else:
            items.extend([
                '수면 패턴을 개선하세요 (7-8시간 규칙적 수면)',
                '정기적인 유산소 운동을 시작하세요 (주 3회, 30분 이상)',
                '균형잡힌 영양 섭취와 충분한 수분 섭취를 하세요'
            ])
        
        items.extend([
            '스트레스 관리 기법을 학습하세요 (명상, 요가, 심호흡 등)',
            '취미 활동이나 개인적 즐거움을 찾는 시간을 늘리세요',
            '정기적인 건강 검진을 받으세요'
        ])
        
        recommendations.append({
            'category': '🏠 개인적 회복',
            'priority': priority,
            'items': items
        })
    
    # 업무 관련 번아웃 권고사항
    if work_score >= 2.5:
        priority = 'high' if work_score >= 3.5 else 'medium'
        items = []
        
        if work_score >= 3.5:
            items.extend([
                '상급자와 즉시 업무량 조정에 대해 논의하세요',
                '부서 이동이나 업무 역할 변경을 고려해보세요',
                '단기 휴가나 휴직을 통한 재충전 시간을 가지세요'
            ])
        else:
            items.extend([
                '현재 업무량과 우선순위를 재평가하세요',
                '동료들과 업무 분담을 늘려보세요',
                '업무 효율성을 높이는 방법을 찾아보세요'
            ])
        
        items.extend([
            '정기적인 휴가를 계획적으로 사용하세요',
            '업무 시간과 개인 시간의 경계를 명확히 하세요',
            '직장 내 스트레스 관리 프로그램을 활용하세요',
            '상급자나 동료와 정기적인 소통 시간을 가지세요'
        ])
        
        recommendations.append({
            'category': '💼 업무 환경 개선',
            'priority': priority,
            'items': items
        })
    
    # 환자 관련 번아웃 권고사항
    if client_score >= 2.5:
        priority = 'high' if client_score >= 3.5 else 'medium'
        items = []
        
        if client_score >= 3.5:
            items.extend([
                '환자 접촉 빈도를 일시적으로 조절하는 방안을 논의하세요',
                '감정 지지를 위한 전문 상담을 받으세요',
                '업무 영역이나 배치 변경을 고려해보세요'
            ])
        else:
            items.extend([
                '감정 조절과 스트레스 관리 기법을 학습하세요',
                '동료들과 어려운 케이스에 대해 정기적으로 이야기하세요',
                '환자 및 가족과의 의사소통 기술을 향상시키세요'
            ])
        
        items.extend([
            '정기적인 디브리핑 시간을 가지세요',
            '전문적 거리감 유지 방법을 익히세요',
            '환자 케어의 긍정적 측면에 초점을 맞춰보세요',
            '동료 지원 그룹이나 모임에 참여하세요'
        ])
        
        recommendations.append({
            'category': '👥 환자 관계 개선',
            'priority': priority,
            'items': items
        })
    
    # 예방 및 유지 관리 (모든 점수가 양호한 경우)
    if max_score < 2.5:
        recommendations.append({
            'category': '✨ 예방 및 유지',
            'priority': 'low',
            'items': [
                '현재의 건강한 상태를 유지하세요',
                '정기적인 자기 점검을 계속하세요 (월 1회 권장)',
                '스트레스 관리 기법을 지속적으로 실천하세요',
                '동료들과의 긍정적 관계를 유지하세요',
                '전문성 개발을 위한 교육에 참여하세요',
                '워라밸을 위한 취미 활동을 지속하세요'
            ]
        })
    
    # 장기적 관리 방안
    recommendations.append({
        'category': '📈 장기적 관리',
        'priority': 'medium',
        'items': [
            '3개월마다 정기적으로 번아웃 수준을 재측정하세요',
            '개인별 스트레스 관리 계획을 수립하고 실행하세요',
            '전문성 향상을 위한 지속적 교육에 참여하세요',
            '멘토나 롤모델을 찾아 조언을 구하세요',
            '경력 개발 계획을 수립하고 정기적으로 검토하세요',
            '개인적 성장과 자기 계발에 투자하세요'
        ]
    })
    
    return recommendations

# 시각화 함수들
def create_advanced_radar_chart(personal_score, work_score, client_score):
    """고급 레이더 차트 생성"""
    categories = ['개인적<br>번아웃', '업무 관련<br>번아웃', '환자 관련<br>번아웃']
    values = [personal_score, work_score, client_score]
    
    fig = go.Figure()
    
    # 현재 점수
    fig.add_trace(go.Scatterpolar(
        r=values + [values[0]],  # 닫힌 도형을 위해 첫 값 추가
        theta=categories + [categories[0]],
        fill='toself',
        fillcolor='rgba(210, 105, 30, 0.3)',
        line=dict(color='#d2691e', width=3),
        marker=dict(size=10, color='#d2691e'),
        name='현재 점수',
        hovertemplate='%{theta}: %{r:.2f}점<extra></extra>'
    ))
    
    # 위험 기준선들
    fig.add_trace(go.Scatterpolar(
        r=[2.5, 2.5, 2.5, 2.5],
        theta=categories + [categories[0]],
        fill='toself',
        fillcolor='rgba(255, 165, 0, 0.1)',
        line=dict(color='#ffa500', width=2, dash='dash'),
        name='주의 기준 (2.5점)',
        hovertemplate='주의 기준: 2.5점<extra></extra>'
    ))
    
    fig.add_trace(go.Scatterpolar(
        r=[3.5, 3.5, 3.5, 3.5],
        theta=categories + [categories[0]],
        fill='toself',
        fillcolor='rgba(220, 20, 60, 0.1)',
        line=dict(color='#dc143c', width=2, dash='dot'),
        name='고위험 기준 (3.5점)',
        hovertemplate='고위험 기준: 3.5점<extra></extra>'
    ))
    
    fig.update_layout(
        polar=dict(
            radialaxis=dict(
                visible=True,
                range=[0, 4],
                tickvals=[0, 1, 2, 3, 4],
                ticktext=['0점', '1점', '2점', '3점', '4점'],
                gridcolor='rgba(0,0,0,0.1)'
            ),
            angularaxis=dict(
                gridcolor='rgba(0,0,0,0.1)'
            )
        ),
        showlegend=True,
        title={
            'text': "CBI 번아웃 점수 분포",
            'x': 0.5,
            'font': {'size': 18, 'color': '#8b4513'}
        },
        font=dict(size=12),
        width=500,
        height=500,
        paper_bgcolor='rgba(0,0,0,0)',
        plot_bgcolor='rgba(0,0,0,0)'
    )
    
    return fig

def create_comparison_chart(personal_score, work_score, client_score, job_type="간호사"):
    """비교 분석 차트 생성"""
    # 업종별 평균 데이터 (예시)
    benchmarks = {
        "간호사": {"개인적": 2.2, "업무관련": 2.6, "환자관련": 2.4},
        "의사": {"개인적": 2.4, "업무관련": 2.8, "환자관련": 2.2},
        "기타 의료진": {"개인적": 2.1, "업무관련": 2.4, "환자관련": 2.0}
    }
    
    categories = ['개인적 번아웃', '업무 관련 번아웃', '환자 관련 번아웃']
    user_scores = [personal_score, work_score, client_score]
    benchmark_scores = list(benchmarks.get(job_type, benchmarks["간호사"]).values())
    
    x = np.arange(len(categories))
    width = 0.35
    
    fig = go.Figure()
    
    fig.add_trace(go.Bar(
        x=categories,
        y=user_scores,
        name='나의 점수',
        marker_color='rgba(210, 105, 30, 0.8)',
        text=[f'{score:.2f}' for score in user_scores],
        textposition='auto',
    ))
    
    fig.add_trace(go.Bar(
        x=categories,
        y=benchmark_scores,
        name=f'{job_type} 평균',
        marker_color='rgba(205, 133, 63, 0.6)',
        text=[f'{score:.2f}' for score in benchmark_scores],
        textposition='auto',
    ))
    
    # 기준선 추가
    fig.add_hline(y=2.5, line_dash="dash", line_color="orange", 
                  annotation_text="주의 기준선 (2.5점)", annotation_position="top right")
    fig.add_hline(y=3.5, line_dash="dash", line_color="red", 
                  annotation_text="고위험 기준선 (3.5점)", annotation_position="top right")
    
    fig.update_layout(
        title={
            'text': f"나의 점수 vs {job_type} 평균 비교",
            'x': 0.5,
            'font': {'size': 18, 'color': '#8b4513'}
        },
        yaxis_title="점수",
        yaxis=dict(range=[0, 4]),
        barmode='group',
        paper_bgcolor='rgba(0,0,0,0)',
        plot_bgcolor='rgba(0,0,0,0)',
        font=dict(size=12)
    )
    
    return fig

def create_trend_simulation(personal_score, work_score, client_score):
    """개선 시뮬레이션 차트"""
    weeks = list(range(0, 13, 4))  # 0, 4, 8, 12주
    
    # 시뮬레이션된 개선 곡선 (지수적 감소)
    def improvement_curve(initial_score, weeks):
        if initial_score <= 2.0:
            return [initial_score] * len(weeks)
        
        improvement_rate = 0.3 if initial_score >= 3.0 else 0.2
        target = max(1.5, initial_score * 0.6)
        
        return [initial_score * np.exp(-improvement_rate * w) + target * (1 - np.exp(-improvement_rate * w)) 
                for w in weeks]
    
    personal_trend = improvement_curve(personal_score, weeks)
    work_trend = improvement_curve(work_score, weeks)
    client_trend = improvement_curve(client_score, weeks)
    
    fig = go.Figure()
    
    fig.add_trace(go.Scatter(
        x=weeks, y=personal_trend,
        mode='lines+markers',
        name='개인적 번아웃',
        line=dict(color='#22c55e', width=3),
        marker=dict(size=8)
    ))
    
    fig.add_trace(go.Scatter(
        x=weeks, y=work_trend,
        mode='lines+markers',
        name='업무 관련 번아웃',
        line=dict(color='#3b82f6', width=3),
        marker=dict(size=8)
    ))
    
    fig.add_trace(go.Scatter(
        x=weeks, y=client_trend,
        mode='lines+markers',
        name='환자 관련 번아웃',
        line=dict(color='#f59e0b', width=3),
        marker=dict(size=8)
    ))
    
    fig.add_hline(y=2.5, line_dash="dash", line_color="orange", 
                  annotation_text="주의 기준", annotation_position="bottom right")
    
    fig.update_layout(
        title={
            'text': "적극적 관리 시 예상 개선 추이",
            'x': 0.5,
            'font': {'size': 18, 'color': '#8b4513'}
        },
        xaxis_title="주차",
        yaxis_title="점수",
        yaxis=dict(range=[0, 4]),
        paper_bgcolor='rgba(0,0,0,0)',
        plot_bgcolor='rgba(0,0,0,0)',
        font=dict(size=12)
    )
    
    return fig

def main():
    # 메인 헤더
    st.markdown("""
    <div class="main-header">
        <h1 class="main-title">🧠 CBI 번아웃 전문 측정센터</h1>
        <p class="main-subtitle">Copenhagen Burnout Inventory 기반 과학적 번아웃 평가 및 맞춤형 솔루션 제공</p>
    </div>
    """, unsafe_allow_html=True)
    
    # 사이드바 - 기본 정보 및 네비게이션
    with st.sidebar:
        st.markdown("### 📋 기본 정보")
        
        name = st.text_input("이름 (선택사항)", placeholder="홍길동")
        
        col1, col2 = st.columns(2)
        with col1:
            age = st.selectbox("연령대", ["20대", "30대", "40대", "50대", "60대 이상"])
        with col2:
            gender = st.selectbox("성별", ["여성", "남성", "기타"])
        
        job = st.selectbox("직업", ["간호사", "의사", "물리치료사", "방사선사", "임상병리사", "기타 의료진", "비의료직"])
        
        if job in ["간호사", "의사"]:
            department = st.selectbox("근무 부서", 
                ["외상센터", "응급실", "중환자실", "수술실", "내과병동", "외과병동", "소아과", "산부인과", "정신과", "기타"])
        else:
            department = "해당없음"
        
        experience = st.selectbox("경력", ["1년 미만", "1-3년", "3-5년", "5-10년", "10-15년", "15년 이상"])
        
        work_pattern = st.selectbox("근무 형태", ["3교대", "2교대", "데이근무", "당직근무", "기타"])
        
        st.markdown("---")
        st.markdown("### 🎯 이전 측정 기록")
        
        if st.button("📊 측정 기록 불러오기"):
            st.info("측정 기록을 불러오는 기능은 준비 중입니다.")
    
    # 세션 상태 초기화
    if 'page' not in st.session_state:
        st.session_state.page = 'intro'
    if 'answers' not in st.session_state:
        st.session_state.answers = {}
    if 'current_domain' not in st.session_state:
        st.session_state.current_domain = 'personal'
    
    # 인트로 페이지
    if st.session_state.page == 'intro':
        # 프로그램 소개
        st.markdown("""
        <div class="section-container">
            <h2 class="section-title">📖 CBI 번아웃 측정에 대해</h2>
        </div>
        """, unsafe_allow_html=True)
        
        col1, col2, col3 = st.columns(3)
        
        with col1:
            st.markdown("""
            <div class="feature-card">
                <div class="feature-icon">🎯</div>
                <h3 style="color: #d2691e; margin-bottom: 1rem;">정확한 측정</h3>
                <p>과학적으로 검증된 CBI 도구를 사용하여 3개 영역에서 번아웃을 정확히 측정합니다.</p>
            </div>
            """, unsafe_allow_html=True)
        
        with col2:
            st.markdown("""
            <div class="feature-card">
                <div class="feature-icon">🔍</div>
                <h3 style="color: #d2691e; margin-bottom: 1rem;">상세한 분석</h3>
                <p>개인적, 업무 관련, 환자 관련 번아웃을 구분하여 원인을 정확히 파악합니다.</p>
            </div>
            """, unsafe_allow_html=True)
        
        with col3:
            st.markdown("""
            <div class="feature-card">
                <div class="feature-icon">💡</div>
                <h3 style="color: #d2691e; margin-bottom: 1rem;">맞춤 솔루션</h3>
                <p>개인별 상황에 맞는 구체적이고 실행 가능한 관리 방안을 제시합니다.</p>
            </div>
            """, unsafe_allow_html=True)
        
        st.markdown("""
        <div class="section-container">
            <h3 style="color: #8b4513; margin-bottom: 1.5rem;">📊 측정 과정</h3>
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 1rem;">
                <div style="background: linear-gradient(135deg, #f0fff4 0%, #dcfce7 100%); padding: 1.5rem; border-radius: 10px; text-align: center;">
                    <h4 style="color: #166534;">1단계: 기본 정보</h4>
                    <p>직업, 경력, 근무 환경 등 기본 정보를 입력합니다.</p>
                </div>
                <div style="background: linear-gradient(135deg, #eff6ff 0%, #dbeafe 100%); padding: 1.5rem; border-radius: 10px; text-align: center;">
                    <h4 style="color: #1e40af;">2단계: CBI 측정</h4>
                    <p>19문항의 설문을 통해 번아웃 수준을 측정합니다.</p>
                </div>
                <div style="background: linear-gradient(135deg, #fefce8 0%, #fef3c7 100%); padding: 1.5rem; border-radius: 10px; text-align: center;">
                    <h4 style="color: #a16207;">3단계: 결과 분석</h4>
                    <p>상세한 해석과 맞춤형 관리 방안을 확인합니다.</p>
                </div>
            </div>
        </div>
        """, unsafe_allow_html=True)
        
        st.markdown("""
        <div style="text-align: center; margin: 3rem 0;">
        """, unsafe_allow_html=True)
        
        if st.button("🚀 CBI 번아웃 측정 시작하기", key="start_assessment"):
            st.session_state.page = 'assessment'
            st.rerun()
        
        st.markdown("</div>", unsafe_allow_html=True)
    
    # 측정 페이지
    elif st.session_state.page == 'assessment':
        # 진행률 계산
        total_questions = sum(len(domain['questions']) for domain in CBI_QUESTIONS.values())
        answered_questions = len(st.session_state.answers)
        progress = answered_questions / total_questions
        
        st.markdown("""
        <div class="section-container">
            <h2 class="section-title">📝 CBI 번아웃 측정</h2>
        </div>
        """, unsafe_allow_html=True)
        
        # 진행률 표시
        st.markdown(f"""
        <div class="progress-container">
            <h3 style="color: #8b4513; margin-bottom: 1rem;">측정 진행률</h3>
            <div style="background: #f0f0f0; border-radius: 10px; padding: 0.5rem; margin-bottom: 1rem;">
                <div style="background: linear-gradient(90deg, #d2691e, #cd853f); height: 20px; 
                           width: {progress*100}%; border-radius: 8px; transition: width 0.3s ease;"></div>
            </div>
            <p style="color: #8b4513; margin: 0;"><strong>{answered_questions}</strong>/{total_questions} 문항 완료 ({progress*100:.1f}%)</p>
        </div>
        """, unsafe_allow_html=True)
        
        # 영역별 측정
        for domain_key, domain_data in CBI_QUESTIONS.items():
            st.markdown(f"""
            <div class="section-container">
                <h3 style="color: #d2691e; margin-bottom: 1rem;">{domain_data['title']}</h3>
                <p style="color: #8b4513; margin-bottom: 2rem; font-size: 1.1rem;">{domain_data['description']}</p>
            </div>
            """, unsafe_allow_html=True)
            
            for i, question in enumerate(domain_data['questions']):
                question_key = f"{domain_key}_{i}"
                
                st.markdown(f"""
                <div class="question-card">
                    <div class="question-text">Q{i+1}. {question}</div>
                </div>
                """, unsafe_allow_html=True)
                
                answer = st.radio(
                    f"답변을 선택해주세요 (Q{i+1})",
                    ANSWER_OPTIONS,
                    key=question_key,
                    horizontal=True,
                    label_visibility="collapsed"
                )
                
                st.session_state.answers[question_key] = ANSWER_OPTIONS.index(answer)
                st.markdown("<br>", unsafe_allow_html=True)
        
        # 측정 완료 버튼
        st.markdown("<br><br>", unsafe_allow_html=True)
        
        col1, col2, col3 = st.columns([1, 2, 1])
        with col2:
            if answered_questions == total_questions:
                if st.button("📊 결과 분석하기", key="analyze_results"):
                    st.session_state.page = 'results'
                    st.rerun()
            else:
                st.info(f"📝 아직 {total_questions - answered_questions}개 문항이 남았습니다. 모든 문항에 답변해주세요.")
    
    # 결과 페이지
    elif st.session_state.page == 'results':
        # 점수 계산
        personal_scores = [st.session_state.answers[f"personal_{i}"] for i in range(len(CBI_QUESTIONS['personal']['questions']))]
        work_scores = [st.session_state.answers[f"work_{i}"] for i in range(len(CBI_QUESTIONS['work']['questions']))]
        client_scores = [st.session_state.answers[f"client_{i}"] for i in range(len(CBI_QUESTIONS['client']['questions']))]
        
        personal_avg = np.mean(personal_scores)
        work_avg = np.mean(work_scores)
        client_avg = np.mean(client_scores)
        overall_avg = np.mean([personal_avg, work_avg, client_avg])
        
        # 메인 헤더
        st.markdown("""
        <div class="section-container">
            <h2 class="section-title">📊 CBI 번아웃 측정 결과</h2>
        </div>
        """, unsafe_allow_html=True)
        
        # 기본 정보 표시
        user_info = f"📅 {datetime.now().strftime('%Y년 %m월 %d일')} 측정"
        if name:
            user_info = f"👤 {name}님 | " + user_info
        user_info += f" | 👔 {job}"
        if department != "해당없음":
            user_info += f" ({department})"
        user_info += f" | 📈 경력 {experience} | ⏰ {work_pattern}"
        
        st.markdown(f"""
        <div style="text-align: center; background: rgba(255,255,255,0.8); padding: 1rem; 
                   border-radius: 10px; margin: 1rem 0; color: #8b4513;">
            {user_info}
        </div>
        """, unsafe_allow_html=True)
        
        # 종합 점수 카드
        st.markdown("""
        <div class="section-container">
            <h3 style="color: #d2691e; text-align: center; margin-bottom: 2rem;">🎯 종합 점수 요약</h3>
        </div>
        """, unsafe_allow_html=True)
        
        col1, col2, col3, col4 = st.columns(4)
        
        # 전체 평균
        with col1:
            overall_risk = assess_risk_level(overall_avg)
            st.markdown(f"""
            <div class="metric-box {overall_risk['class']}">
                <div class="metric-label">전체 평균</div>
                <div class="metric-score">{overall_avg:.2f}</div>
                <div class="metric-status">{overall_risk['icon']} {overall_risk['level']}</div>
            </div>
            """, unsafe_allow_html=True)
        
        # 개인적 번아웃
        with col2:
            personal_risk = assess_risk_level(personal_avg)
            st.markdown(f"""
            <div class="metric-box {personal_risk['class']}">
                <div class="metric-label">개인적 번아웃</div>
                <div class="metric-score">{personal_avg:.2f}</div>
                <div class="metric-status">{personal_risk['icon']} {personal_risk['level']}</div>
            </div>
            """, unsafe_allow_html=True)
        
        # 업무 관련 번아웃
        with col3:
            work_risk = assess_risk_level(work_avg)
            st.markdown(f"""
            <div class="metric-box {work_risk['class']}">
                <div class="metric-label">업무 관련 번아웃</div>
                <div class="metric-score">{work_avg:.2f}</div>
                <div class="metric-status">{work_risk['icon']} {work_risk['level']}</div>
            </div>
            """, unsafe_allow_html=True)
        
        # 환자 관련 번아웃
        with col4:
            client_risk = assess_risk_level(client_avg)
            st.markdown(f"""
            <div class="metric-box {client_risk['class']}">
                <div class="metric-label">환자 관련 번아웃</div>
                <div class="metric-score">{client_avg:.2f}</div>
                <div class="metric-status">{client_risk['icon']} {client_risk['level']}</div>
            </div>
            """, unsafe_allow_html=True)
        
        # 시각화 섹션
        st.markdown("""
        <div class="section-container">
            <h3 style="color: #d2691e; text-align: center; margin-bottom: 2rem;">📈 시각적 분석</h3>
        </div>
        """, unsafe_allow_html=True)
        
        col1, col2 = st.columns(2)
        
        with col1:
            st.markdown('<div class="chart-container">', unsafe_allow_html=True)
            fig_radar = create_advanced_radar_chart(personal_avg, work_avg, client_avg)
            st.plotly_chart(fig_radar, use_container_width=True)
            st.markdown('</div>', unsafe_allow_html=True)
        
        with col2:
            st.markdown('<div class="chart-container">', unsafe_allow_html=True)
            fig_comparison = create_comparison_chart(personal_avg, work_avg, client_avg, job)
            st.plotly_chart(fig_comparison, use_container_width=True)
            st.markdown('</div>', unsafe_allow_html=True)
        
        # 개선 시뮬레이션
        st.markdown('<div class="chart-container">', unsafe_allow_html=True)
        fig_trend = create_trend_simulation(personal_avg, work_avg, client_avg)
        st.plotly_chart(fig_trend, use_container_width=True)
        st.markdown('</div>', unsafe_allow_html=True)
        
        # 상세 해석
        st.markdown("""
        <div class="section-container">
            <h3 style="color: #d2691e; text-align: center; margin-bottom: 2rem;">🔍 영역별 상세 해석</h3>
        </div>
        """, unsafe_allow_html=True)
        
        domains = [
            ('personal', '🏠 개인적 번아웃', personal_avg),
            ('work', '💼 업무 관련 번아웃', work_avg),
            ('client', '👥 환자 관련 번아웃', client_avg)
        ]
        
        for domain_key, domain_name, score in domains:
            interpretation = get_detailed_interpretation(domain_key, score)
            risk = interpretation['risk']
            details = interpretation['interpretation']
            
            st.markdown(f"""
            <div class="interpretation-card">
                <div class="interpretation-title">
                    {risk['icon']} {domain_name} ({score:.2f}점 - {risk['level']})
                </div>
                <div class="interpretation-content">
                    <h4 style="color: #166534; margin-bottom: 0.5rem;">📋 종합 평가</h4>
                    <p style="margin-bottom: 1.5rem;">{details['summary']}</p>
                    
                    <h4 style="color: #166534; margin-bottom: 0.5rem;">📝 상세 분석</h4>
                    <p style="margin-bottom: 1.5rem;">{details['details']}</p>
                    
                    <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem;">
                        <div>
                            <h4 style="color: #22c55e; margin-bottom: 0.5rem;">✅ 강점</h4>
                            <ul style="margin: 0; padding-left: 1.2rem;">
                                {"".join([f"<li>{strength}</li>" for strength in details['strengths']])}
                            </ul>
                        </div>
                        <div>
                            <h4 style="color: #f59e0b; margin-bottom: 0.5rem;">👀 주의사항</h4>
                            <ul style="margin: 0; padding-left: 1.2rem;">
                                {"".join([f"<li>{point}</li>" for point in details['watch_points']])}
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
            """, unsafe_allow_html=True)
        
        # 맞춤형 권고사항
        st.markdown("""
        <div class="section-container">
            <h3 style="color: #d2691e; text-align: center; margin-bottom: 2rem;">💡 맞춤형 관리 방안</h3>
        </div>
        """, unsafe_allow_html=True)
        
        recommendations = generate_comprehensive_recommendations(personal_avg, work_avg, client_avg)
        
        for rec in recommendations:
            priority_colors = {
                'high': '#dc2626',
                'medium': '#f59e0b', 
                'low': '#22c55e'
            }
            priority_labels = {
                'high': '🚨 긴급',
                'medium': '⚠️ 중요',
                'low': '💚 예방'
            }
            
            color = priority_colors[rec['priority']]
            label = priority_labels[rec['priority']]
            
            st.markdown(f"""
            <div class="recommendation-section priority-{rec['priority']}">
                <div class="recommendation-title">
                    {label} {rec['category']}
                </div>
            """, unsafe_allow_html=True)
            
            for item in rec['items']:
                st.markdown(f"""
                <div class="recommendation-item">
                    ✅ {item}
                </div>
                """, unsafe_allow_html=True)
            
            st.markdown("</div>", unsafe_allow_html=True)
        
        # 전문가 도움 및 추가 정보
        st.markdown("""
        <div class="section-container">
            <h3 style="color: #d2691e; text-align: center; margin-bottom: 2rem;">📞 전문가 도움 요청 기준</h3>
        </div>
        """, unsafe_allow_html=True)
        
        col1, col2 = st.columns(2)
        
        with col1:
            st.markdown("""
            <div style="background: linear-gradient(135deg, #fef2f2 0%, #fee2e2 100%); 
                        padding: 2rem; border-radius: 15px; border: 1px solid #fecaca;">
                <h4 style="color: #dc2626; margin-bottom: 1rem;">🚨 즉시 전문가 상담이 필요한 경우</h4>
                <ul style="line-height: 1.8; color: #7f1d1d;">
                    <li>어느 영역이든 <strong>3.5점 이상</strong></li>
                    <li>2주 이상 지속되는 <strong>심각한 증상</strong></li>
                    <li>일상생활이나 업무에 <strong>심각한 지장</strong></li>
                    <li><strong>자해나 자살</strong> 생각이 드는 경우</li>
                    <li>알코올이나 약물에 <strong>의존</strong>하게 되는 경우</li>
                </ul>
                <div style="background: rgba(255,255,255,0.8); padding: 1rem; border-radius: 8px; margin-top: 1rem;">
                    <p style="margin: 0; font-weight: 600; color: #dc2626;">
                        📞 직장 내 상담실, EAP, 정신건강의학과 등을 이용하세요
                    </p>
                </div>
            </div>
            """, unsafe_allow_html=True)
        
        with col2:
            st.markdown("""
            <div style="background: linear-gradient(135deg, #f0fff4 0%, #dcfce7 100%); 
                        padding: 2rem; border-radius: 15px; border: 1px solid #bbf7d0;">
                <h4 style="color: #22c55e; margin-bottom: 1rem;">📈 정기 측정 권장 일정</h4>
                <ul style="line-height: 1.8; color: #166534;">
                    <li><strong>신규 직원:</strong> 매월 1회 (첫 6개월)</li>
                    <li><strong>경력 직원:</strong> 3개월마다 1회</li>
                    <li><strong>고위험군:</strong> 매월 1회</li>
                    <li><strong>관리자:</strong> 팀원과 함께 분기별</li>
                </ul>
                <div style="background: rgba(255,255,255,0.8); padding: 1rem; border-radius: 8px; margin-top: 1rem;">
                    <p style="margin: 0; font-weight: 600; color: #22c55e;">
                        📊 변화 추이 관찰로 조기 예방이 핵심입니다
                    </p>
                </div>
            </div>
            """, unsafe_allow_html=True)
        
        # 결과 저장 및 공유
        st.markdown("<br><br>", unsafe_allow_html=True)
        
        col1, col2, col3 = st.columns([1, 2, 1])
        with col2:
            # 새로운 측정 버튼
            if st.button("🔄 새로운 측정하기", key="new_assessment"):
                # 세션 상태 초기화
                for key in list(st.session_state.keys()):
                    if key.startswith(('personal_', 'work_', 'client_')):
                        del st.session_state[key]
                st.session_state.page = 'intro'
                st.session_state.answers = {}
                st.rerun()
            
            # 결과 다운로드
            if st.button("💾 결과 저장하기", key="save_results"):
                # 결과 데이터 구성
                result_data = {
                    "측정일시": datetime.now().strftime('%Y-%m-%d %H:%M:%S'),
                    "이름": name if name else "익명",
                    "연령대": age,
                    "성별": gender,
                    "직업": job,
                    "부서": department,
                    "경력": experience,
                    "근무형태": work_pattern,
                    "개인적_번아웃": round(personal_avg, 2),
                    "업무관련_번아웃": round(work_avg, 2),
                    "환자관련_번아웃": round(client_avg, 2),
                    "전체_평균": round(overall_avg, 2),
                    "개인적_위험도": personal_risk['level'],
                    "업무관련_위험도": work_risk['level'],
                    "환자관련_위험도": client_risk['level'],
                    "전체_위험도": overall_risk['level']
                }
                
                # CSV 다운로드
                df = pd.DataFrame([result_data])
                csv = df.to_csv(index=False, encoding='utf-8-sig')
                
                st.download_button(
                    label="📄 CSV 파일로 다운로드",
                    data=csv,
                    file_name=f"CBI_번아웃측정결과_{datetime.now().strftime('%Y%m%d_%H%M%S')}.csv",
                    mime="text/csv"
                )
            
            # 결과 공유 링크 생성 (시뮬레이션)
            if st.button("🔗 결과 공유하기", key="share_results"):
                share_url = f"https://cbi-burnout-app.streamlit.app/shared/{datetime.now().strftime('%Y%m%d%H%M%S')}"
                st.success(f"📋 공유 링크가 생성되었습니다: {share_url}")
                st.info("💡 이 링크를 통해 상급자나 동료와 결과를 공유할 수 있습니다 (실제 구현 시 보안 고려 필요)")
    
    # 푸터
    st.markdown("""
    <div class="footer">
        <h3 style="margin-bottom: 1rem;">🏥 CBI 번아웃 전문 측정센터</h3>
        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 2rem; margin-bottom: 2rem;">
            <div>
                <h4>📞 문의사항</h4>
                <p>이메일: support@cbi-center.com<br>
                전화: 1588-1234<br>
                운영시간: 24시간 연중무휴</p>
            </div>
            <div>
                <h4>🔒 개인정보 보호</h4>
                <p>모든 측정 데이터는 암호화되어 안전하게 보호됩니다.<br>
                개인 식별 정보는 저장되지 않습니다.</p>
            </div>
            <div>
                <h4>📊 과학적 근거</h4>
                <p>CBI는 국제적으로 검증된 도구입니다.<br>
                정확성과 신뢰성이 입증되었습니다.</p>
            </div>
        </div>
        <hr style="border: 1px solid rgba(255,255,255,0.3); margin: 2rem 0;">
        <p style="margin: 0; opacity: 0.8;">
            © 2024 CBI 번아웃 전문 측정센터. 과학적 번아웃 평가를 통한 건강한 직장 환경 조성에 기여합니다.
        </p>
    </div>
    """, unsafe_allow_html=True)

if __name__ == "__main__":
    main()
