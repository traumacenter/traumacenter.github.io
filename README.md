import streamlit as st
import pandas as pd
import numpy as np
import plotly.graph_objects as go
import plotly.express as px
from datetime import datetime
import base64

# 페이지 설정
st.set_page_config(
    page_title="CBI 번아웃 측정 프로그램",
    page_icon="🧠",
    layout="wide",
    initial_sidebar_state="collapsed"
)

# CSS 스타일링 - 따뜻하고 전문적인 디자인
st.markdown("""
<style>
    .main {
        padding-top: 2rem;
    }
    
    .stApp {
        background: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%);
    }
    
    .main-container {
        background: white;
        padding: 2rem;
        border-radius: 20px;
        box-shadow: 0 10px 30px rgba(0,0,0,0.1);
        margin: 1rem;
    }
    
    .header-title {
        color: #d2691e;
        font-size: 3rem;
        font-weight: 700;
        text-align: center;
        margin-bottom: 1rem;
        text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
    }
    
    .header-subtitle {
        color: #8b4513;
        font-size: 1.2rem;
        text-align: center;
        margin-bottom: 2rem;
        font-weight: 300;
    }
    
    .section-header {
        color: #cd853f;
        font-size: 1.8rem;
        font-weight: 600;
        margin: 2rem 0 1rem 0;
        border-bottom: 3px solid #deb887;
        padding-bottom: 0.5rem;
    }
    
    .metric-card {
        background: linear-gradient(135deg, #fff8dc 0%, #f5deb3 100%);
        padding: 1.5rem;
        border-radius: 15px;
        border-left: 5px solid #d2691e;
        margin: 1rem 0;
        box-shadow: 0 4px 15px rgba(0,0,0,0.05);
    }
    
    .risk-low {
        border-left-color: #32cd32;
        background: linear-gradient(135deg, #f0fff0 0%, #e6ffe6 100%);
    }
    
    .risk-medium {
        border-left-color: #ffa500;
        background: linear-gradient(135deg, #fff8dc 0%, #ffeaa7 100%);
    }
    
    .risk-high {
        border-left-color: #dc143c;
        background: linear-gradient(135deg, #ffe4e1 0%, #ffcccb 100%);
    }
    
    .recommendation-box {
        background: linear-gradient(135deg, #e8f5e8 0%, #d4edda 100%);
        padding: 1.5rem;
        border-radius: 15px;
        border: 1px solid #c3e6cb;
        margin: 1rem 0;
    }
    
    .stButton > button {
        background: linear-gradient(135deg, #d2691e 0%, #cd853f 100%);
        color: white;
        border: none;
        padding: 0.75rem 2rem;
        border-radius: 25px;
        font-weight: 600;
        font-size: 1rem;
        transition: all 0.3s ease;
    }
    
    .stButton > button:hover {
        transform: translateY(-2px);
        box-shadow: 0 5px 15px rgba(210, 105, 30, 0.3);
    }
    
    .question-container {
        background: #fafafa;
        padding: 1.5rem;
        border-radius: 10px;
        margin: 1rem 0;
        border-left: 4px solid #d2691e;
    }
</style>
""", unsafe_allow_html=True)

# CBI 문항 정의
CBI_QUESTIONS = {
    'personal': [
        "얼마나 자주 피곤함을 느끼십니까?",
        "얼마나 자주 신체적으로 탈진감을 느끼십니까?",
        "얼마나 자주 정서적으로 탈진감을 느끼십니까?",
        "얼마나 자주 '더 이상 견딜 수 없다'고 생각하십니까?",
        "얼마나 자주 지친다고 느끼십니까?",
        "얼마나 자주 몸이 약해졌다고 느끼십니까?"
    ],
    'work': [
        "일로 인해 얼마나 자주 지치십니까?",
        "일로 인해 얼마나 자주 신체적으로 탈진감을 느끼십니까?",
        "일로 인해 얼마나 자주 정서적으로 탈진감을 느끼십니까?",
        "일로 인해 얼마나 자주 좌절감을 느끼십니까?",
        "일로 인해 얼마나 자주 완전히 지쳐버린다고 느끼십니까?",
        "일에 대해 생각하는 것이 얼마나 자주 스트레스가 됩니까?",
        "아침에 일어나서 또 하루 일해야 한다고 생각하면 얼마나 자주 지치십니까?"
    ],
    'client': [
        "환자들과 일하는 것이 얼마나 자주 스트레스가 됩니까?",
        "환자들과 일한 후 얼마나 자주 지치십니까?",
        "환자들과 일하는 것이 얼마나 자주 좌절스럽습니까?",
        "환자들과 일할 때 얼마나 자주 한계를 느끼십니까?",
        "환자들과 일하는 것이 얼마나 자주 정서적으로 힘듭니까?",
        "환자들에게 에너지를 주는 것이 얼마나 자주 힘듭니까?"
    ]
}

ANSWER_OPTIONS = [
    "전혀 그렇지 않다",
    "거의 그렇지 않다", 
    "가끔 그렇다",
    "자주 그렇다",
    "항상 그렇다"
]

def get_risk_level(score):
    """점수에 따른 위험도 반환"""
    if score < 2.0:
        return "낮음", "risk-low", "#32cd32"
    elif score < 2.5:
        return "경계", "risk-medium", "#ffa500"
    elif score < 3.5:
        return "보통", "risk-medium", "#ffa500"
    else:
        return "높음", "risk-high", "#dc143c"

def interpret_score(domain, score):
    """영역별 점수 해석"""
    risk_level, _, _ = get_risk_level(score)
    
    interpretations = {
        'personal': {
            '낮음': "기본적인 에너지 수준이 양호하며, 일상생활에서 활력감을 잘 유지하고 있습니다.",
            '경계': "전반적인 피로감이 약간 증가하고 있습니다. 예방적 관리가 필요합니다.",
            '보통': "전반적인 피로감이 증가하고 있으며, 휴식 후에도 완전한 회복이 어려울 수 있습니다.",
            '높음': "심각한 전반적 탈진 상태로, 기본적인 일상생활도 힘든 상황입니다."
        },
        'work': {
            '낮음': "업무 환경에 잘 적응하고 있으며, 업무 스트레스를 효과적으로 관리하고 있습니다.",
            '경계': "업무 스트레스가 약간 누적되고 있습니다. 주의 깊은 관찰이 필요합니다.",
            '보통': "업무 스트레스가 누적되어 업무로 인한 피로감이 증가하고 있습니다.",
            '높음': "업무로 인한 심각한 탈진 상태로, 출근 자체가 큰 스트레스가 되고 있습니다."
        },
        'client': {
            '낮음': "환자와의 관계에서 만족감을 느끼며, 공감 능력을 잘 유지하고 있습니다.",
            '경계': "환자 관계에서 약간의 스트레스가 나타나기 시작했습니다.",
            '보통': "환자 관계에서 스트레스가 증가하고 있으며, 공감 피로가 시작되었습니다.",
            '높음': "환자와의 관계에서 심한 탈진을 느끼며, 공감 능력이 현저히 감소했습니다."
        }
    }
    
    return interpretations[domain][risk_level]

def get_recommendations(personal_score, work_score, client_score):
    """맞춤형 권고사항 생성"""
    recommendations = []
    
    # 개인적 번아웃 권고사항
    if personal_score >= 3.5:
        recommendations.append({
            "영역": "개인적 번아웃 (긴급)",
            "권고사항": [
                "즉시 의료진 상담을 받으시기 바랍니다",
                "장기간 휴식을 고려해보세요",
                "근본적인 생활 방식 변화가 필요합니다",
                "전문적인 심리 상담을 받는 것을 권장합니다"
            ],
            "우선순위": "높음"
        })
    elif personal_score >= 2.0:
        recommendations.append({
            "영역": "개인적 번아웃",
            "권고사항": [
                "수면 패턴을 개선하세요 (7-8시간 수면)",
                "정기적인 운동을 시작하세요 (주 3회, 30분)",
                "스트레스 관리 기법을 도입하세요 (명상, 요가 등)",
                "영양 균형을 맞춘 식사를 하세요"
            ],
            "우선순위": "중간"
        })
    
    # 업무 관련 번아웃 권고사항
    if work_score >= 3.5:
        recommendations.append({
            "영역": "업무 관련 번아웃 (긴급)",
            "권고사항": [
                "상급자와 즉시 업무 조정을 논의하세요",
                "부서 이동이나 휴직을 고려해보세요",
                "직장 내 상담실을 이용하세요",
                "업무량 재분배를 요청하세요"
            ],
            "우선순위": "높음"
        })
    elif work_score >= 2.0:
        recommendations.append({
            "영역": "업무 관련 번아웃",
            "권고사항": [
                "업무량 조절을 상급자와 협의하세요",
                "동료와의 업무 분담을 늘리세요",
                "업무 효율성 개선 방법을 찾아보세요",
                "정기적인 휴가를 활용하세요"
            ],
            "우선순위": "중간"
        })
    
    # 환자 관련 번아웃 권고사항
    if client_score >= 3.5:
        recommendations.append({
            "영역": "환자 관련 번아웃 (긴급)",
            "권고사항": [
                "환자 접촉 빈도를 일시적으로 조절하세요",
                "감정 지지 상담을 받으세요",
                "업무 영역을 재조정해보세요",
                "동료들과 어려운 사례를 공유하세요"
            ],
            "우선순위": "높음"
        })
    elif client_score >= 2.0:
        recommendations.append({
            "영역": "환자 관련 번아웃",
            "권고사항": [
                "감정 조절 기법을 학습하세요",
                "동료와의 경험 공유 시간을 가지세요",
                "환자 관계 개선 교육에 참여하세요",
                "디브리핑 시간을 정기적으로 가지세요"
            ],
            "우선순위": "중간"
        })
    
    # 전체적으로 양호한 경우
    if not recommendations:
        recommendations.append({
            "영역": "예방 관리",
            "권고사항": [
                "현재의 건강한 상태를 유지하세요",
                "정기적인 자기 점검을 계속하세요",
                "스트레스 관리 기법을 지속적으로 실천하세요",
                "동료들과의 긍정적 관계를 유지하세요"
            ],
            "우선순위": "낮음"
        })
    
    return recommendations

def create_radar_chart(personal_score, work_score, client_score):
    """레이더 차트 생성"""
    categories = ['개인적 번아웃', '업무 관련 번아웃', '환자 관련 번아웃']
    values = [personal_score, work_score, client_score]
    
    fig = go.Figure()
    
    fig.add_trace(go.Scatterpolar(
        r=values,
        theta=categories,
        fill='toself',
        fillcolor='rgba(210, 105, 30, 0.3)',
        line=dict(color='#d2691e', width=3),
        marker=dict(size=8, color='#d2691e'),
        name='현재 점수'
    ))
    
    # 기준선 추가
    fig.add_trace(go.Scatterpolar(
        r=[2.5, 2.5, 2.5],
        theta=categories,
        fill='toself',
        fillcolor='rgba(255, 165, 0, 0.1)',
        line=dict(color='#ffa500', width=2, dash='dash'),
        name='주의 기준선 (2.5점)'
    ))
    
    fig.update_layout(
        polar=dict(
            radialaxis=dict(
                visible=True,
                range=[0, 4],
                tickvals=[1, 2, 3, 4],
                ticktext=['1점', '2점', '3점', '4점']
            )
        ),
        showlegend=True,
        title="CBI 번아웃 점수 분포",
        font=dict(size=14),
        width=500,
        height=500
    )
    
    return fig

def main():
    # 메인 헤더
    st.markdown('<div class="main-container">', unsafe_allow_html=True)
    st.markdown('<h1 class="header-title">🧠 CBI 번아웃 측정 프로그램</h1>', unsafe_allow_html=True)
    st.markdown('<p class="header-subtitle">Copenhagen Burnout Inventory를 통한 전문적인 번아웃 평가</p>', unsafe_allow_html=True)
    
    # 사이드바 - 기본 정보
    with st.sidebar:
        st.markdown("### 📋 기본 정보")
        name = st.text_input("이름 (선택사항)", placeholder="익명으로도 가능합니다")
        job = st.selectbox("직업", ["간호사", "의사", "기타 의료진", "기타"])
        if job == "간호사":
            department = st.selectbox("근무 부서", ["외상센터", "응급실", "중환자실", "일반병동", "기타"])
        else:
            department = "해당없음"
        experience = st.selectbox("경력", ["1년 미만", "1-3년", "3-5년", "5-10년", "10년 이상"])
    
    # 초기 상태 설정
    if 'current_page' not in st.session_state:
        st.session_state.current_page = 'intro'
    if 'answers' not in st.session_state:
        st.session_state.answers = {}
    
    # 시작 페이지
    if st.session_state.current_page == 'intro':
        st.markdown('<h2 class="section-header">📖 CBI 번아웃 측정이란?</h2>', unsafe_allow_html=True)
        
        col1, col2 = st.columns([2, 1])
        
        with col1:
            st.markdown("""
            <div style="background: #f8f9fa; padding: 1.5rem; border-radius: 10px; margin: 1rem 0;">
            <h3 style="color: #d2691e; margin-bottom: 1rem;">🎯 측정 목적</h3>
            <p style="line-height: 1.6;">
            CBI는 개인적, 업무 관련, 환자 관련 3개 영역에서 번아웃 수준을 측정하여 
            구체적인 원인을 파악하고 맞춤형 관리 방안을 제시합니다.
            </p>
            </div>
            """, unsafe_allow_html=True)
            
            st.markdown("""
            <div style="background: #e8f5e8; padding: 1.5rem; border-radius: 10px; margin: 1rem 0;">
            <h3 style="color: #d2691e; margin-bottom: 1rem;">📊 측정 방법</h3>
            <ul style="line-height: 1.8;">
                <li><strong>총 19문항</strong>을 5점 척도로 평가</li>
                <li><strong>약 5-10분</strong> 소요</li>
                <li><strong>즉시 결과</strong> 확인 및 분석</li>
                <li><strong>맞춤형 권고사항</strong> 제공</li>
            </ul>
            </div>
            """, unsafe_allow_html=True)
        
        with col2:
            st.markdown("""
            <div style="background: linear-gradient(135deg, #fff8dc 0%, #f5deb3 100%); 
                        padding: 1.5rem; border-radius: 15px; text-align: center;">
                <h3 style="color: #d2691e; margin-bottom: 1rem;">⏱️ 소요 시간</h3>
                <div style="font-size: 2.5rem; color: #cd853f; font-weight: bold;">5-10분</div>
                <p style="margin-top: 1rem; color: #8b4513;">정확한 측정을 위해<br>신중하게 답해주세요</p>
            </div>
            """, unsafe_allow_html=True)
        
        st.markdown("<br>", unsafe_allow_html=True)
        
        if st.button("🚀 측정 시작하기", key="start_button"):
            st.session_state.current_page = 'assessment'
            st.rerun()
    
    # 측정 페이지
    elif st.session_state.current_page == 'assessment':
        st.markdown('<h2 class="section-header">📝 CBI 번아웃 측정</h2>', unsafe_allow_html=True)
        
        # 진행률 표시
        total_questions = len(CBI_QUESTIONS['personal']) + len(CBI_QUESTIONS['work']) + len(CBI_QUESTIONS['client'])
        current_answers = len(st.session_state.answers)
        progress = current_answers / total_questions
        
        st.progress(progress)
        st.markdown(f"<p style='text-align: center; color: #8b4513;'>진행률: {current_answers}/{total_questions} ({progress*100:.1f}%)</p>", unsafe_allow_html=True)
        
        # 개인적 번아웃 영역
        st.markdown('<h3 style="color: #d2691e; margin-top: 2rem;">🏠 개인적 번아웃 (6문항)</h3>', unsafe_allow_html=True)
        st.markdown('<p style="color: #8b4513; margin-bottom: 1rem;">일과 관계없는 전반적인 피로와 탈진 정도를 평가합니다.</p>', unsafe_allow_html=True)
        
        for i, question in enumerate(CBI_QUESTIONS['personal']):
            st.markdown(f'<div class="question-container">', unsafe_allow_html=True)
            st.markdown(f'<p style="font-weight: 600; color: #d2691e;">Q{i+1}. {question}</p>', unsafe_allow_html=True)
            answer = st.radio(
                f"답변 선택 (Q{i+1})",
                ANSWER_OPTIONS,
                key=f"personal_{i}",
                label_visibility="collapsed",
                horizontal=True
            )
            st.session_state.answers[f"personal_{i}"] = ANSWER_OPTIONS.index(answer)
            st.markdown('</div>', unsafe_allow_html=True)
        
        # 업무 관련 번아웃 영역
        st.markdown('<h3 style="color: #d2691e; margin-top: 2rem;">💼 업무 관련 번아웃 (7문항)</h3>', unsafe_allow_html=True)
        st.markdown('<p style="color: #8b4513; margin-bottom: 1rem;">직접적으로 업무로 인한 피로와 탈진 정도를 평가합니다.</p>', unsafe_allow_html=True)
        
        for i, question in enumerate(CBI_QUESTIONS['work']):
            st.markdown(f'<div class="question-container">', unsafe_allow_html=True)
            st.markdown(f'<p style="font-weight: 600; color: #d2691e;">Q{i+7}. {question}</p>', unsafe_allow_html=True)
            answer = st.radio(
                f"답변 선택 (Q{i+7})",
                ANSWER_OPTIONS,
                key=f"work_{i}",
                label_visibility="collapsed",
                horizontal=True
            )
            st.session_state.answers[f"work_{i}"] = ANSWER_OPTIONS.index(answer)
            st.markdown('</div>', unsafe_allow_html=True)
        
        # 환자 관련 번아웃 영역  
        st.markdown('<h3 style="color: #d2691e; margin-top: 2rem;">👥 환자 관련 번아웃 (6문항)</h3>', unsafe_allow_html=True)
        st.markdown('<p style="color: #8b4513; margin-bottom: 1rem;">환자나 고객과의 상호작용에서 오는 탈진 정도를 평가합니다.</p>', unsafe_allow_html=True)
        
        for i, question in enumerate(CBI_QUESTIONS['client']):
            st.markdown(f'<div class="question-container">', unsafe_allow_html=True)
            st.markdown(f'<p style="font-weight: 600; color: #d2691e;">Q{i+14}. {question}</p>', unsafe_allow_html=True)
            answer = st.radio(
                f"답변 선택 (Q{i+14})",
                ANSWER_OPTIONS,
                key=f"client_{i}",
                label_visibility="collapsed",
                horizontal=True
            )
            st.session_state.answers[f"client_{i}"] = ANSWER_OPTIONS.index(answer)
            st.markdown('</div>', unsafe_allow_html=True)
        
        st.markdown("<br><br>", unsafe_allow_html=True)
        
        col1, col2, col3 = st.columns([1, 2, 1])
        with col2:
            if st.button("📊 결과 분석하기", key="analyze_button"):
                if len(st.session_state.answers) == total_questions:
                    st.session_state.current_page = 'results'
                    st.rerun()
                else:
                    st.error("모든 문항에 답변해주세요.")
    
    # 결과 페이지
    elif st.session_state.current_page == 'results':
        # 점수 계산
        personal_scores = [st.session_state.answers[f"personal_{i}"] for i in range(len(CBI_QUESTIONS['personal']))]
        work_scores = [st.session_state.answers[f"work_{i}"] for i in range(len(CBI_QUESTIONS['work']))]
        client_scores = [st.session_state.answers[f"client_{i}"] for i in range(len(CBI_QUESTIONS['client']))]
        
        personal_avg = np.mean(personal_scores)
        work_avg = np.mean(work_scores)
        client_avg = np.mean(client_scores)
        overall_avg = np.mean([personal_avg, work_avg, client_avg])
        
        st.markdown('<h2 class="section-header">📊 CBI 번아웃 측정 결과</h2>', unsafe_allow_html=True)
        
        # 기본 정보 표시
        if name:
            st.markdown(f"<p style='text-align: center; font-size: 1.2rem; color: #8b4513;'>👤 {name}님의 측정 결과</p>", unsafe_allow_html=True)
        
        info_text = f"📅 측정일: {datetime.now().strftime('%Y년 %m월 %d일')} | 👔 직업: {job}"
        if department != "해당없음":
            info_text += f" ({department})"
        info_text += f" | 📈 경력: {experience}"
        st.markdown(f"<p style='text-align: center; color: #8b4513; margin-bottom: 2rem;'>{info_text}</p>", unsafe_allow_html=True)
        
        # 전체 점수 요약
        st.markdown('<h3 style="color: #d2691e;">🎯 종합 점수</h3>', unsafe_allow_html=True)
        
        col1, col2, col3, col4 = st.columns(4)
        
        with col1:
            risk_level, risk_class, risk_color = get_risk_level(overall_avg)
            st.markdown(f"""
            <div class="metric-card {risk_class}">
                <h4 style="margin: 0; color: {risk_color};">전체 평균</h4>
                <div style="font-size: 2rem; font-weight: bold; color: {risk_color};">{overall_avg:.2f}점</div>
                <p style="margin: 0; color: {risk_color};">{risk_level} 위험</p>
            </div>
            """, unsafe_allow_html=True)
        
        with col2:
            risk_level, risk_class, risk_color = get_risk_level(personal_avg)
            st.markdown(f"""
            <div class="metric-card {risk_class}">
                <h4 style="margin: 0; color: {risk_color};">개인적 번아웃</h4>
                <div style="font-size: 2rem; font-weight: bold; color: {risk_color};">{personal_avg:.2f}점</div>
                <p style="margin: 0; color: {risk_color};">{risk_level} 위험</p>
            </div>
            """, unsafe_allow_html=True)
        
        with col3:
            risk_level, risk_class, risk_color = get_risk_level(work_avg)
            st.markdown(f"""
            <div class="metric-card {risk_class}">
                <h4 style="margin: 0; color: {risk_color};">업무 관련 번아웃</h4>
                <div style="font-size: 2rem; font-weight: bold; color: {risk_color};">{work_avg:.2f}점</div>
                <p style="margin: 0; color: {risk_color};">{risk_level} 위험</p>
            </div>
            """, unsafe_allow_html=True)
        
        with col4:
            risk_level, risk_class, risk_color = get_risk_level(client_avg)
            st.markdown(f"""
            <div class="metric-card {risk_class}">
                <h4 style="margin: 0; color: {risk_color};">환자 관련 번아웃</h4>
                <div style="font-size: 2rem; font-weight: bold; color: {risk_color};">{client_avg:.2f}점</div>
                <p style="margin: 0; color: {risk_color};">{risk_level} 위험</p>
            </div>
            """, unsafe_allow_html=True)
        
        # 시각화
        st.markdown('<h3 style="color: #d2691e; margin-top: 2rem;">📈 시각적 분석</h3>', unsafe_allow_html=True)
        
        col1, col2 = st.columns([1, 1])
        
        with col1:
            # 레이더 차트
            fig_radar = create_radar_chart(personal_avg, work_avg, client_avg)
            st.plotly_chart(fig_radar, use_container_width=True)
        
        with col2:
            # 막대 차트
            fig_bar = px.bar(
                x=['개인적', '업무관련', '환자관련'],
                y=[personal_avg, work_avg, client_avg],
                color=[personal_avg, work_avg, client_avg],
                color_continuous_scale=['#32cd32', '#ffa500', '#dc143c'],
                title="영역별 번아웃 점수 비교"
            )
            fig_bar.add_hline(y=2.5, line_dash="dash", line_color="orange", 
                             annotation_text="주의 기준선 (2.5점)")
            fig_bar.add_hline(y=3.5, line_dash="dash", line_color="red", 
                             annotation_text="고위험 기준선 (3.5점)")
            fig_bar.update_layout(showlegend=False, yaxis_range=[0, 4])
            fig_bar.update_yaxis(title="점수")
            fig_bar.update_xaxis(title="번아웃 영역")
            st.plotly_chart(fig_bar, use_container_width=True)
        
        # 상세 해석
        st.markdown('<h3 style="color: #d2691e; margin-top: 2rem;">🔍 상세 해석</h3>', unsafe_allow_html=True)
        
        domains = [
            ('개인적 번아웃', 'personal', personal_avg),
            ('업무 관련 번아웃', 'work', work_avg),
            ('환자 관련 번아웃', 'client', client_avg)
        ]
        
        for domain_name, domain_key, score in domains:
            risk_level, risk_class, risk_color = get_risk_level(score)
            interpretation = interpret_score(domain_key, score)
            
            st.markdown(f"""
            <div class="metric-card {risk_class}">
                <h4 style="color: {risk_color}; margin-bottom: 1rem;">📋 {domain_name} ({score:.2f}점 - {risk_level} 위험)</h4>
                <p style="line-height: 1.6; margin: 0;">{interpretation}</p>
            </div>
            """, unsafe_allow_html=True)
        
        # 맞춤형 권고사항
        st.markdown('<h3 style="color: #d2691e; margin-top: 2rem;">💡 맞춤형 권고사항</h3>', unsafe_allow_html=True)
        
        recommendations = get_recommendations(personal_avg, work_avg, client_avg)
        
        for rec in recommendations:
            priority_color = {"높음": "#dc143c", "중간": "#ffa500", "낮음": "#32cd32"}[rec["우선순위"]]
            
            st.markdown(f"""
            <div class="recommendation-box">
                <h4 style="color: {priority_color}; margin-bottom: 1rem;">
                    🎯 {rec["영역"]} (우선순위: {rec["우선순위"]})
                </h4>
            """, unsafe_allow_html=True)
            
            for i, item in enumerate(rec["권고사항"], 1):
                st.markdown(f"<p style='margin: 0.5rem 0; line-height: 1.6;'>✅ {item}</p>", unsafe_allow_html=True)
            
            st.markdown("</div>", unsafe_allow_html=True)
        
        # 추가 정보
        st.markdown('<h3 style="color: #d2691e; margin-top: 2rem;">📚 추가 정보</h3>', unsafe_allow_html=True)
        
        col1, col2 = st.columns(2)
        
        with col1:
            st.markdown("""
            <div style="background: #f0f8ff; padding: 1.5rem; border-radius: 10px; border-left: 4px solid #4169e1;">
                <h4 style="color: #4169e1; margin-bottom: 1rem;">📞 전문가 도움이 필요한 경우</h4>
                <ul style="line-height: 1.8;">
                    <li>어느 영역이든 <strong>3.5점 이상</strong></li>
                    <li>2주 이상 <strong>지속되는 증상</strong></li>
                    <li>일상생활에 <strong>심각한 지장</strong></li>
                    <li>자해나 자살 생각</li>
                </ul>
                <p style="margin-top: 1rem; font-weight: 600; color: #4169e1;">
                    🏥 직장 내 상담실 또는 정신건강 전문의 상담을 받으시기 바랍니다.
                </p>
            </div>
            """, unsafe_allow_html=True)
        
        with col2:
            st.markdown("""
            <div style="background: #f5f5dc; padding: 1.5rem; border-radius: 10px; border-left: 4px solid #daa520;">
                <h4 style="color: #daa520; margin-bottom: 1rem;">📅 정기 측정 권장</h4>
                <ul style="line-height: 1.8;">
                    <li><strong>신규간호사:</strong> 매월 1회 (첫 6개월)</li>
                    <li><strong>경력간호사:</strong> 분기별 1회</li>
                    <li><strong>고위험군:</strong> 매월 1회</li>
                </ul>
                <p style="margin-top: 1rem; font-weight: 600; color: #daa520;">
                    📈 변화 추이를 관찰하여 조기 개입하는 것이 중요합니다.
                </p>
            </div>
            """, unsafe_allow_html=True)
        
        st.markdown("<br><br>", unsafe_allow_html=True)
        
        # 버튼들
        col1, col2, col3 = st.columns([1, 2, 1])
        with col2:
            if st.button("🔄 다시 측정하기", key="restart_button"):
                st.session_state.clear()
                st.rerun()
            
            # 결과 다운로드
            if st.button("💾 결과 저장하기", key="download_button"):
                result_data = {
                    "측정일": datetime.now().strftime('%Y년 %m월 %d일 %H시 %M분'),
                    "이름": name if name else "익명",
                    "직업": job,
                    "부서": department,
                    "경력": experience,
                    "개인적_번아웃": f"{personal_avg:.2f}점",
                    "업무관련_번아웃": f"{work_avg:.2f}점", 
                    "환자관련_번아웃": f"{client_avg:.2f}점",
                    "전체_평균": f"{overall_avg:.2f}점"
                }
                
                df = pd.DataFrame([result_data])
                csv = df.to_csv(index=False, encoding='utf-8-sig')
                
                st.download_button(
                    label="📄 CSV 파일로 다운로드",
                    data=csv,
                    file_name=f"CBI_번아웃측정결과_{datetime.now().strftime('%Y%m%d_%H%M%S')}.csv",
                    mime="text/csv"
                )
    
    st.markdown('</div>', unsafe_allow_html=True)
    
    # 푸터
    st.markdown("<br><br>", unsafe_allow_html=True)
    st.markdown("""
    <div style="text-align: center; padding: 2rem; background: #f8f9fa; border-radius: 10px; margin-top: 2rem;">
        <p style="color: #8b4513; margin: 0;">
            📧 문의사항이 있으시면 언제든 연락해주세요 | 
            🔒 모든 데이터는 안전하게 보호됩니다 | 
            📋 CBI는 과학적으로 검증된 측정 도구입니다
        </p>
    </div>
    """, unsafe_allow_html=True)

if __name__ == "__main__":
    main()
