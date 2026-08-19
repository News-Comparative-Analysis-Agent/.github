# 📰 FOCUS — 멀티에이전트 비평기사 생성 AI (2026.02 ~ 2026.05)

<div align="center">

> **연계 기업**: 미디어스 (미디어 비평 전문 매체)

> 뉴스 수집부터 초안까지 — 오픈소스 LLM(Qwen3)을 자체 호스팅한 6단계 멀티에이전트 파이프라인으로 미디어 비평 기사를 자동 생성하는 시스템

<img width="2068" height="780" alt="FOCUS 소개 배너" src="https://github.com/user-attachments/assets/e02c4af5-9d88-46f0-a84d-e6354dd32310" />
<img width="2144" height="1104" alt="FOCUS 시스템 흐름도" src="https://github.com/user-attachments/assets/9f4b97fd-184a-40c9-80da-9eeb27210b7e" />

</div>

기자 관점에서 시스템은 **당일 이슈 파악 → 심층 분석 → 초안 작성 → 최종 검토** 네 단계로 동작하며, 각 단계를 아래의 에이전트 파이프라인이 뒷받침합니다.

# 👥 팀원 소개 (Team Members)

| <b>박영호</b> | <b>임창수</b> | <b>백지원</b> |
| :---: | :---: | :---: |
| 팀장 | 팀원 · 백엔드 | 팀원 · 프론트엔드 |
| 인프라 구축<br/>에이전트 개발<br/>프롬프트 고도화 | 에이전트 개발<br/>클러스터링 & 크롤링 고도화 | UI/UX 담당 |

# 📅 프로젝트 개요

- **진행 기간**: 26.02 ~ 26.05
- **연계 기업**: 미디어스 (미디어 비평 전문 매체)
- **개발 배경**:
  미디어스 기자는 매일 당일 이슈를 파악하고, 해당 이슈를 다룬 언론사별 논조를 비교한 뒤 기사 초안을 작성하는 데 많은 시간을 쓰고 있었습니다.
  이를 개선하기 위해 기사 수집부터 언론사별 논조 비교, 초안 작성까지 이어지는 AI 에이전트 시스템을 구축하여 기자가 기사 작성 자체에 집중할 수 있도록 했습니다.

## 에이전트 파이프라인 (Agent Pipeline)

6개의 전문 에이전트가 순차적으로 협력하여 하나의 기사를 완성합니다. Judge Agent가 초안을 불합격 판정하면 Writer Agent로 되돌려 재생성합니다.

```
[Scout] → [Cluster] → [Evidence] → [Issue] → [Writer] ⇄ [Judge]
                                              (미달 시 재생성 루프)
```

## 에이전트별 역할

| # | 에이전트 | 역할 |
| :---: | :--- | :--- |
| 1 | **Scout Agent** | 키워드/분야 기반 최신 뉴스 기사 수집 (크롤링) |
| 2 | **Cluster Agent** | TF-IDF + SBERT 하이브리드 유사도 기반 기사 군집화 및 이슈 그룹화 |
| 3 | **Evidence Agent** | 이슈에 속한 기사들의 논조 분석, 핵심 주장과 객관적 근거 추출 |
| 4 | **Issue Agent** | 언론사 간 대립 지점 분석, 이슈의 배경·맥락·핵심 쟁점 요약 |
| 5 | **Writer Agent** | 이슈 요약을 바탕으로 미디어스 스타일 비평 기사 초안 작성 |
| 6 | **Judge Agent** | 초안 품질 평가 및 합격 판정, 미달 시 재생성 트리거 |

| **기존 방식** | **멀티에이전트 파이프라인** |
| :---: | :---: |
| 수동 뉴스 수집 및 분석 | **자동 크롤링 → 클러스터링 → 기사 생성** |
| 기자가 직접 근거 검색 | **Evidence Agent가 자동 근거 추출** |
| 기자가 직접 쟁점 파악 | **Issue Agent가 주요 쟁점 추출** |
| 기자가 직접 초안 생성 | **Writer Agent가 자동 초안 생성** |
| 주관적 편집 판단 | **Judge Agent가 품질 자동 검증** |

<br>

---
<br>

## 🏗️ 시스템 아키텍처 (Architecture)

<img width="1946" height="934" alt="시스템 아키텍처 다이어그램" src="https://github.com/user-attachments/assets/5410b4cd-81cf-4756-aeb5-23642b527105" />

<br>

## 🛠️ 기술 스택 (Tech Stack)

| Category | Tech Stack | Details |
| :--- | :--- | :--- |
| **Language & Framework** | ![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white) | Python 3.11, FastAPI 0.109 |
| **LLM & Agent** | ![Qwen](https://img.shields.io/badge/Qwen3--8B-8E75B2?logo=alibabacloud&logoColor=white) ![LangGraph](https://img.shields.io/badge/LangGraph-1C3C3C?logo=langchain&logoColor=white) | Qwen3-8B (자체 호스팅) · LangGraph 기반 멀티에이전트 오케스트레이션 |
| **Data & NLP** | ![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikit-learn&logoColor=white) ![SBERT](https://img.shields.io/badge/SBERT-Embedding-4B8BBE) | TF-IDF + SBERT 임베딩 가중 결합 하이브리드 군집화<br/>KoNLPy, BeautifulSoup4 |
| **Database** | ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white) | PostgreSQL (SQLAlchemy + Alembic) |
| **Frontend** | ![React](https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=black) ![Vercel](https://img.shields.io/badge/Vercel-000000?logo=vercel&logoColor=white) | React (TypeScript), Vercel 배포 |
| **Infra** | ![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white) ![DigitalOcean](https://img.shields.io/badge/DigitalOcean-0080FF?logo=digitalocean&logoColor=white) | Docker Compose · DigitalOcean Droplets & App Platform |

## 🔍 하이브리드 군집화 알고리즘 (Hybrid Clustering)

어휘적 정확성과 문맥적 유연성을 모두 확보하기 위해, 기사 간 유사도를 **TF-IDF 유사도**와 **SBERT 임베딩 유사도**의 가중 결합으로 계산합니다.

```
Similarity(dᵢ, dⱼ) = α · Sim_TF-IDF(dᵢ, dⱼ) + (1 − α) · Sim_SBERT(dᵢ, dⱼ)
```

<img alt="하이브리드 군집화 알고리즘 흐름" src="https://github.com/user-attachments/assets/3938d437-1da9-4a32-af2c-87908ba35171" />

## 📊 평가 결과 (Evaluation Results)

Judge Agent는 생성된 기사를 실제 미디어스 기사와 비교하여 유사도를 측정합니다.

### 평가 지표

| 지표 | 설명 |
| :--- | :--- |
| **문체 유사도** | 미디어스 특유의 비평 문체 재현 정도 |
| **구조 유사도** | 제목 포맷, 단락 구성, 인용 방식 일치도 |
| **내용 충실도** | 근거 활용 정확성 및 논지 일관성 |
| **종합 유사도** | 위 3개 지표의 가중 평균 |

### 개선 이력

| 라운드 | 주요 변경사항 | 종합 유사도 |
| :---: | :--- | :---: |
| v1 | 기본 파이프라인 구축 | ~36% |
| v2 | 프롬프트 엔지니어링 1차 개선 | ~50% |
| v3 | 제목 포맷 재설계, 신문사별 인용 한도 제한 | ~65% |
| v4 | 날짜 포맷 후처리, 이슈 배경 히스토리 전략 개선 | ~80% |
| **v5 (최종)** | **Qwen3-8B / Gemma4-31B 비교 실험 → Qwen3-8B 채택** | **~80%+** |

## 🚀 한계점 및 향후 개선 사항 (Limitations & Future Improvements)

### 1. 이슈 군집화(Clustering) 정확도 한계

- **현재 한계**: 유사도 계산이 제목에 높은 가중치를 두기 때문에, 의미가 다른 기사라도 "~를 해결하기 위해 ~했다"처럼 제목 형태가 비슷하면 같은 이슈로 묶이거나, 반대로 하나의 이슈가 여러 군집으로 과분할되는 문제가 있습니다.
- **향후 계획**: 기사에서 추출한 핵심 개체명(인명·기관명·지명 등 고유명사, NER)을 활용해, 개체명이 하나도 겹치지 않는 기사 쌍은 유사도 점수가 높더라도 동일 군집으로 묶이지 않도록 필터링을 추가할 계획입니다.

### 2. 문체 유사도 고도화

- 현재 종합 유사도 약 80% 수준으로, 목표인 **95% 이상** 달성을 위해 프롬프트 고도화 및 모델 비교 실험을 이어갈 예정입니다.
