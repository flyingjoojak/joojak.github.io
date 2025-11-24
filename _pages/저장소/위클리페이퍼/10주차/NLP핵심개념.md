---
title: "📚 NLP 핵심 개념 정리 — 전처리, FastText, Attention, Transformer"
tags:
    - NLP
    - 딥러닝
    - AI
date: "2025-11-16"
thumbnail: "/assets/img/thumbnail/NLP.png"
---

# 📚 NLP 핵심 개념 정리 — 전처리, FastText, Attention, Transformer

---

## 1. 텍스트 데이터를 모델에 적용하기 전 필요한 전처리 과정

텍스트는 숫자가 아니기 때문에, 모델이 이해할 수 있도록 변환해야 합니다. 주요 단계는 다음과 같습니다:

### 데이터 정제(Cleaning)
- 불필요한 문자 제거(특수기호, HTML 태그 등)
- 대소문자 통일(lowercasing)
- 불용어(stopwords) 제거
- 중복 데이터 제거

### 토큰화(Tokenization)
- 문장을 단어 또는 서브워드 단위로 분리
- 예: WordPiece, SentencePiece, BPE(Byte-Pair Encoding)

### 정규화(Normalization)
- 표제어 추출(Lemmatization)
- 어간 추출(Stemming)

### 숫자 변환(Vectorization/Encoding)
- One-hot encoding
- TF-IDF
- Word2Vec / FastText / GloVe
- BERT, GPT 등 Transformer 기반 임베딩

### 패딩 및 길이 통일(Padding)
- 시퀀스 길이를 동일하게 맞추기 위해 `<PAD>` 토큰 추가

### 토큰 인덱스 매핑
- 단어 → 정수 ID로 변환
- vocabulary 생성

---

## 2. FastText와 Word2Vec의 차이점 및 FastText의 장점

### Word2Vec 특징
- 단어 단위 임베딩
- 단어 전체를 하나의 벡터로 표현
- 의미 기반 학습(CBOW, Skip-gram)

### FastText 특징
- Facebook AI가 개발
- 단어를 **n-gram 문자(subword)** 단위로 분해해 학습
- 예: “apple” → “ap”, “app”, “ppl”, “ple”

### FastText의 장점
1. **희소 단어(OOV) 처리 가능**
   - Word2Vec은 단어 사전에 없는 단어 처리 불가
   - FastText는 subword 조합으로 벡터 생성

2. **형태 변화에 강함**
   - “run”, “running”, “runner” 간 유사 관계 유지

3. **희귀 단어 학습 성능 개선**
   - 적은 등장 빈도여도 subword로 표현 가능

4. **언어적 특성이 강한 한국어 등에 유리**
   - 조사, 어미 변화 처리 가능

---

## 3. Attention 메커니즘이 Seq2Seq 모델의 어떤 문제를 해결하는가?

전통적인 RNN 기반 Seq2Seq 모델의 단점:
- 모든 입력 정보를 **하나의 고정 길이 벡터**(context vector)에 압축
- 입력 길이가 길수록 정보 손실 증가
- 장기 의존성(long-term dependency) 학습 어려움

### Attention이 해결한 부분
- 인코더의 모든 hidden state를 디코더가 참고 가능
- 각 생성 시점마다 **중요한 부분에 가중치 할당**
- 사람이 문장을 읽을 때 특정 단어에 집중하는 방식과 유사
- 긴 문장, 번역, 요약 등에 큰 성능 향상

### 핵심 효과
- 정보 압축 문제 완화
- 장기 의존성 개선
- 문맥 이해력 증가
- 학습 안정화

---

## 4. Transformer 모델과 Seq2Seq의 근본적 차이

### 전통 Seq2Seq (RNN/LSTM/GRU 기반)
- 순차적(sequence) 처리 필요
- 병렬 처리 어려움
- 의존성 거리가 길수록 정보 손실 증가

### Transformer
- **전부 Attention 기반 모델**
- RNN/Convolution 사용하지 않음
- 입력 토큰을 병렬 처리 가능 → 빠른 학습
- Self-Attention으로 전체 문맥을 한 번에 고려

### 구조 비교

| 요소 | 기존 Seq2Seq | Transformer |
|------|--------------|-------------|
| 기본 연산 | RNN/LSTM/GRU | Self-Attention |
| 입력 처리 방식 | 순차적 | 병렬 |
| 긴 문맥 처리 | 어려움 | 매우 강함 |
| 학습 속도 | 느림 | 빠름 |
| 구조 형태 | Encoder–Decoder | Encoder–Decoder (but attention 중심) |

### 핵심 차이 요약
- Transformer는 순차 의존성을 제거하고 병렬화를 가능하게 함
- Attention만으로 언어 이해 및 생성 가능
- 대규모 데이터와 GPU 병렬 처리에 최적화

---

## 결론 요약

- 텍스트 전처리는 모델 성능을 좌우하는 핵심 단계
- FastText는 subword를 사용해 Word2Vec의 OOV 문제 해결
- Attention은 Seq2Seq의 정보 압축·장기 의존성 문제를 완화
- Transformer는 Attention 기반 병렬 구조로 NLP 혁신을 이끎