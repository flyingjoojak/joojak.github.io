---
title: "📃 BERT vs GPT vs 최신 사전학습 모델 & Hugging Face 정리"
tags:
    - NLP
    - 모델
    - 딥러닝
date: "2025-11-24"
thumbnail: "/assets/img/thumbnail/BERT.jpg"
---

# 📃 BERT vs GPT vs 최신 사전학습 모델 & Hugging Face 정리

---

## 1. BERT와 GPT의 주요 차이점

### BERT와 GPT의 개념
- **BERT**: Bidirectional Encoder Representations from Transformers  
- **GPT**: Generative Pre-trained Transformer

---

### 📚 기본 구조 비교

| 항목 | BERT | GPT |
|------|------|-----|
| 모델 구조 | Transformer **Encoder-only** | Transformer **Decoder-only** |
| 문맥 처리 방향 | **양방향 (Bidirectional)** | **단방향 (Left → Right)** |
| 학습 방식 | Masked Language Modeling(MLM), NSP | Autoregressive LM(다음 토큰 예측) |
| 주요 목적 | 텍스트 **이해(NLU)** | 텍스트 **생성(NLG)** |

---

### 📚 작동 방식

#### BERT
- 중간 단어를 `[MASK]`로 가리고 맞히도록 학습  
- 양쪽 문맥을 모두 활용 → 의미 파악·추론에 강함

#### GPT
- 이전 토큰들을 기반으로 다음 단어를 생성  
- 자연스럽고 연속적인 문장 생성에 최적화

---

### 📚 적합한 NLP 응용 분야

#### BERT가 유리한 작업
- 감성 분석
- 문장 분류
- 텍스트 유사도
- 개체명 인식(NER)
- 질의응답(QA)
- 정보 추출

#### GPT가 유리한 작업
- 글쓰기·요약·번역
- 질의응답 챗봇
- 스토리/콘텐츠 생성
- 코드 생성
- 자동 보고서 작성

---

## 2. Hugging Face Transformers 라이브러리란?

### 📚 정의
- NLP·CV·음성 모델을 쉽게 사용할 수 있도록 제공하는 **오픈소스 모델 허브 & 라이브러리**
- PyTorch, TensorFlow, JAX 지원

---

### 📚 주요 기능

| 기능 | 설명 |
|------|------|
| Model Hub | 수천 개의 사전학습 모델 제공 |
| Pipeline API | 한 줄 코드로 inference 수행 |
| Tokenizers | 빠르고 효율적인 서브워드 토크나이저 |
| Trainer | 분산 학습·mixed precision·fine-tuning |
| Framework 호환 | PyTorch / TensorFlow / ONNX |
| 커뮤니티 | 모델 공유·데이터셋·Spaces 제공 |

---

### 📚 사용 예시

```python
from transformers import pipeline

classifier = pipeline("sentiment-analysis")
result = classifier("I love transformers!")
print(result)
```

```python
from transformers import AutoModelForSequenceClassification, AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")
model = AutoModelForSequenceClassification.from_pretrained("bert-base-uncased")
```

---

## 3. BERT·GPT 이후 등장한 주요 사전학습 모델

### RoBERTa
- BERT 개선 버전
- NSP 제거, 더 큰 데이터·학습 시간
- NLU 성능 향상

### ELECTRA
- MLM 대신 **Replaced Token Detection**
- 훨씬 적은 비용으로 높은 성능

### XLNet
- Permutation-based autoregressive training
- 양방향 정보 활용 + 생성 능력 결합

### DeBERTa / DeBERTa V3
- Disentangled attention 구조
- SOTA급 NLU 성능

### T5 (Text-to-Text Transfer Transformer)
- 모든 NLP task를 텍스트 입력 → 텍스트 출력으로 통일
- 번역·요약·QA 등 다양하게 활용

### BART
- Encoder–Decoder 구조
- 요약·복원·생성 작업에 강함

### GPT-3·GPT-4 및 LLaMA 계열
- 초대형 생성 모델
- few-shot & zero-shot 능력
- 대규모 언어 생성/추론에 강력

---

## 4. 핵심 정리

- **BERT = 이해 중심**
- **GPT = 생성 중심**
- **Transformers 라이브러리 = 모델 사용을 위한 표준 생태계**
- **이후 모델들은 효율성·확장성·긴 문맥 처리·성능 개선 방향으로 진화**

