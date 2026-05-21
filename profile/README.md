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

| Category | Stack | Version / Details |
| :--- | :--- | :--- |
| **Language & Framework** | ![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white) | Python 3.11 |
| **LLM** | ![DeepInfra](https://img.shields.io/badge/DeepInfra-API-FF6B35?logoColor=white) | Qwen3-32B / Gemma4-31B (비교 실험) |
| **Vector DB** | ![Qdrant](https://img.shields.io/badge/Qdrant-1.x-E91E63?logo=qdrant&logoColor=white) | 이슈 배경 이벤트 스트림 저장 |
| **Embedding** | — | 한국어 임베딩 모델 |
| **Infra** | ![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white) | Docker Compose |

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



### 크롤링 정책

| 항목 | 정책 |
| :--- | :--- |
| **신문사별 인용 한도** | 특정 신문사 편중 방지를 위해 신문사당 최대 N개 제한 |
| **날짜 포맷** | Python 후처리로 `YYYY년 MM월 DD일` 통일 |
| **중복 제거** | Cluster Agent에서 유사도 기반 중복 기사 필터링 |

<br>

## 🚀 한계점 및 향후 개선 사항 (Limitations & Future Improvements)

### 1. 모델 선택 최적화 (Model Selection)
- **현재 한계**: Qwen3-32B vs Gemma4-31B 간 성능 비교 실험 진행 중이며, 에이전트별 최적 모델이 미확정
- **향후 계획**: 에이전트별로 역할에 맞는 모델을 분리 적용하는 **에이전트-모델 매핑 전략** 도입 예정

### 2. 유사도 평가 고도화 (Evaluation Enhancement)
- **현재 한계**: 유사도 측정 기준이 단순 텍스트 비교에 의존하며, 미디어 비평의 논조·관점 반영이 미흡
- **향후 계획**: 문체·논조·구조를 분리 측정하는 **다차원 평가 지표** 도입 및 자동화 파이프라인 구축

### 3. 실시간 뉴스 대응 (Real-time News)
- **현재 한계**: 배치 단위 크롤링으로 실시간 이슈 대응이 지연됨
- **향후 계획**: 뉴스 이벤트 스트리밍 기반 **트리거형 파이프라인** 구축으로 이슈 발생 즉시 기사 생성 가능하도록 개선

### 4. 환각(Hallucination) 억제 (Hallucination Mitigation)
- **현재 한계**: LLM 생성 과정에서 근거에 없는 내용이 포함될 수 있음
- **향후 계획**: Evidence Agent 기반 **팩트체킹 레이어** 추가 및 Judge Agent 검증 항목에 사실관계 확인 포함
