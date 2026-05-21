# 📰 멀티에이전트 기사 생성 파이프라인 (Multi-Agent Article Generation Pipeline)

<div align="center">

> **"미디어스 스타일의 미디어 비평 기사를 자동으로"**


> 뉴스 크롤링부터 이슈 분석, 기사 생성, 품질 검증까지 — 6단계 에이전트 파이프라인으로 실제 미디어 비평 기사를 자동 생성하는 시스템
<img width="2068" height="780" alt="image" src="https://github.com/user-attachments/assets/e02c4af5-9d88-46f0-a84d-e6354dd32310" />
<img width="2144" height="1104" alt="image" src="https://github.com/user-attachments/assets/9f4b97fd-184a-40c9-80da-9eeb27210b7e" />


</div>

<br>

## 📅 프로젝트 개요 (Project Overview)

- **진행 기간**: 2026.02 ~ 2026.05
- **연계 기업**: 미디어스 (미디어 비평 전문 매체)
- **개발 목적**:  
  미디어 비평 기사 작성에 필요한 뉴스 수집, 이슈 분석, 근거 추출, 기사 작성 과정을 **멀티에이전트 파이프라인으로 자동화**하여, 실제 미디어스 기사와 유사한 스타일·구조의 비평 기사를 생성합니다.

<br>

| **기존 방식** | **멀티에이전트 파이프라인** |
| :---: | :---: |
| 수동 뉴스 수집 및 분석 | **자동 크롤링 → 클러스터링 → 기사 생성** |
| 기자가 직접 근거 검색 | **Evidence Agent가 자동 근거 추출** |
| 기자가 직접 초안 생성 | **Writer Agent가 자동 초안 생성** |
| 주관적 편집 판단 | **Judge Agent가 품질 자동 검증** |

<br>

## 🤖 에이전트 파이프라인 (Agent Pipeline)

6개의 전문 에이전트가 순차적으로 협력하여 하나의 기사를 완성합니다.

```
[Crawl] → [Cluster] → [Evidence] → [Issue] → [Writer] → [Judge]
```

### 에이전트별 역할

| # | 에이전트 | 역할 |
| :---: | :--- | :--- |
| 1 | **Crawl Agent** | 키워드/분야 기반 최신 뉴스 기사 수집 | 
| 2 | **Cluster Agent** | 유사 기사 클러스터링 및 이슈 그룹화 | 
| 3 | **Evidence Agent** | 특정 이슈에 속한 기사들에서 핵심 주장과 객관적 근거 추출 | 
| 4 | **Issue Agent** | 추출된 근거들을 바탕으로 이슈의 배경, 맥락, 핵심 쟁점 요약 |
| 5 | **Writer Agent** | 이슈 요약을 바탕으로 미디어스 스타일 비평 기사 초안 작성 | 
| 6 | **Judge Agent** | 초안 평가 및 합격 판정, 미달 시 재생성 트리거  |

<br>

<br>

## 🏗️ 시스템 아키텍처 (Architecture)

<img width="1946" height="934" alt="image" src="https://github.com/user-attachments/assets/5410b4cd-81cf-4756-aeb5-23642b527105" />


<br>

## 🛠️ 기술 스택 (Tech Stack)

| Category | Tech Stack | Details |
| :--- | :--- | :--- |
| **Language & Framework** | ![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white) | Python 3.11, FastAPI 0.109 |
| **LLM & Agent** | ![Qwen](https://img.shields.io/badge/Qwen3-8E75B2?logo=aliyun&logoColor=white) <br> ![LangGraph](https://img.shields.io/badge/LangGraph-1C3C3C?logo=langchain&logoColor=white) | Qwen3-8B <br> LangGraph 기반 멀티 에이전트 파이프라인 |
| **Database** | ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white) | PostgreSQL (SQLAlchemy + Alembic ORM) |
| **Data & NLP** | ![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikit-learn&logoColor=white) | scikit-learn (TF-IDF 코사인 유사도 기반 군집화) <br> KoNLPy, BeautifulSoup4 |
| **Infra** | ![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white) ![DigitalOcean](https://img.shields.io/badge/DigitalOcean-0080FF?logo=digitalocean&logoColor=white) | Docker Compose <br> DigitalOcean Droplets & App Platform |
<br>

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
| **현재** | **Qwen3-32B / Gemma4-31B 비교 실험 진행 중** | **~80%+** |

> 목표: **90% 이상** 유사도 달성

<br>

## 💾 비용 설계 (Data Design)

### 이슈 배경 히스토리 전략

이슈의 맥락 유지를 위해 **증분 요약(Incremental Summary) + Qdrant 이벤트 스트림** 하이브리드 전략을 채택했습니다.


<br>

## 🚀 한계점 및 향후 개선 사항 (Limitations & Future Improvements)

### 1. 이슈 군집화(Clustering) 정확도 한계
- **현재 한계**: TF-IDF와 코사인 유사도에 기반한 단순 어휘(Lexical) 기반 클러스터링을 사용하다 보니, 표면적인 단어만 겹치고 실제 맥락(의미)은 전혀 다른 기사들이 무의미하게 묶이는 현상이 약 20~30% 비율로 발생하고 있습니다.
- **향후 계획**: 단순 단어 빈도수 매칭을 넘어 기사의 내포된 맥락을 이해할 수 있도록 관련 논문을 살펴 해당 기능을 보완할 수 있는 방법론을 찾아 도입하고, 완벽하게 클러스터링 할 수 있도록 검증단계까지 거치는 레이어를 추가할 예정입니다.




