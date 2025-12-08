---
title: "📃 RAG & LangChain 핵심 정리 — 구성 요소, 평가 방법, Agent 개념"
tags:
    - LLM
    - RAG
    - 모델
date: "2025-12-08"
thumbnail: "/assets/img/thumbnail/RAG.png"
---

# 📃 RAG & LangChain 핵심 정리 — 구성 요소, 평가 방법, Agent 개념

---

## 1. LangChain 기반 RAG 시스템 구축 시 필요한 주요 구성 요소

RAG(Retrieval-Augmented Generation)는 **검색 + 생성 모델**을 결합해 더 정확한 답변을 제공하는 구조입니다. LangChain에서는 다음과 같은 요소들이 필요합니다:

### 1. Document Loader
- 외부 데이터를 불러오는 모듈  
- 예: PDF, 웹페이지, 텍스트 파일, DB 등  
- 역할: 원본 지식을 RAG 시스템에 공급

### 2. Text Splitter
- 문서를 적절한 길이로 나누는 도구  
- 예: RecursiveCharacterTextSplitter  
- 역할: 검색 효율 향상 및 문맥 유지

### 3. Embedding Model
- 텍스트를 벡터로 변환하는 모델  
- 예: OpenAI Embeddings, SentenceTransformers  
- 역할: 유사한 텍스트를 찾기 위한 수치화 과정

### 4. Vector Store
- 벡터를 저장하고 검색하는 데이터베이스  
- 예: FAISS, Pinecone, Chroma  
- 역할: 질문과 가장 관련 있는 문서를 빠르게 찾기

### 5. Retriever
- 벡터 스토어에서 문서를 검색하는 모듈  
- 역할: 질문과 관련성 높은 문서를 LLM에 제공

### 6. LLM (Generator)
- 검색된 문서를 기반으로 답변을 생성  
- 역할: 내용을 요약, 분석, 결합하여 자연스러운 언어로 응답 생성

### 7. RAG Chain (Retrieval + Generation)
- LangChain의 핵심 구조  
- 역할: Retriever → LLM을 연결하여 문서 기반 답변을 자동 생성

---

## 2. RAG 시스템의 성능 평가 방법

RAG 성능 평가는 **독립 평가(offline)**와 **종단간 평가(end-to-end)**로 나뉩니다.

---

### 1. 독립 평가 (Component-wise / Offline Evaluation)

검색 성능을 개별적으로 평가하는 방식.

#### 주요 지표
- **Precision@k**  
- **Recall@k**  
- **Retrieval Accuracy**  
- **Embedding 품질 테스트**  
- **검색 속도(latency)**  

#### 목적
- 검색 품질이 전체 시스템 성능의 병목이 되지 않도록 측정  
- LLM을 포함하지 않음

---

### 2. 종단간 평가 (End-to-End Evaluation)

검색 + 생성 전체 파이프라인을 평가하는 방식.

#### 주요 지표
- **Answer Correctness**  
- **Faithfulness (근거 충실성)**  
- **Hallucination Rate**  
- **Groundedness Score**  

---

### 독립 평가 vs 종단간 평가 차이

| 평가 방식 | 초점 | 평가 범위 |
|-----------|------|-----------|
| 독립 평가 | 검색 품질 | Retriever만 평가 |
| 종단간 평가 | 최종 출력 품질 | Retriever + LLM 전체 |

---

## 3. RAG 시스템에서 Agent란 무엇이며 어떻게 구현되는가?

### Agent의 개념
Agent는 LLM이 **스스로 판단하여 도구를 선택하고 문제를 해결하는 자동화된 시스템**이다.

RAG 환경에서 Agent는 다음을 수행할 수 있다:

- 검색이 필요한지 자체적으로 판단  
- 여러 번 검색 후 내용 비교  
- 문서를 기반으로 답변 생성  
- 계산기, 외부 API 같은 도구 활용  
- 결과를 검증하고 필요 시 다시 수행  

---

## Agent의 역할

1. **도구 선택**  
2. **계획 생성(Planning)**  
3. **반복 추론(ReAct)**  
4. **정답 검증(Self-checking)**  

---

## Agent 구현 방식 (LangChain 기준)

### 1. ReAct 패턴 (Reason + Act)
- LLM이 “생각(추론) → 행동(도구 호출) → 관찰(결과)” 루프를 수행  
- 검색 도구, DB, 계산기 등 다양한 Tool 자동 활용

### 2. Tool Calling 기반 Agent
- OpenAI·Anthropic·Google 모델이 지원  
- JSON Schema 기반으로 필요한 도구를 선택  
- LangChain이 자동 실행 후 결과를 다시 LLM에 전달

### 3. Multi-step RAG Agent
- 필요한 경우 여러 번 검색하고 판단 후 최종 답변 생성  
- 복잡한 질문에서 높은 정확도 제공

### 4. Self-RAG
- LLM이 스스로 "검색이 필요한지" 결정  
- 검색 결과의 품질을 점검하고 필요하면 추가 검색  
- 할루시네이션을 크게 줄이는 방식

---

## 결론 요약

- LangChain RAG는 Loader, Splitter, Embedding, Vector Store, Retriever, LLM, RAG Chain으로 구성된다.  
- 평가 방식은 **독립 평가(검색 품질만 측정)**와 **종단간 평가(전체 파이프라인 측정)**로 구분된다.  
- Agent는 도구를 사용하며 복잡한 작업을 스스로 해결하는 LLM 구조로, ReAct·Tool Calling·Self-RAG 방식으로 구현할 수 있다.


