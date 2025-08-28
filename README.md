## 프로젝트 개요

- Seq2Seq Q&A Chatbot 구현 프로젝트입니다.
- 질문-답변 쌍의 데이터를 학습하여 사용자의 질문에 대한 적절한 답변을 생성합니다.
- Seq2Seq 모델과 SentencePiece 토크나이저를 활용했습니다.

## 데이터

- 데이터 소스: ChatbotData.csv 파일 사용
- 형식: 'Q' (질문), 'A' (답변) 컬럼으로 구성된 CSV
- 전처리:
   - SentencePiece를 이용한 토큰화 및 단어 집합 생성
   - 질문 및 답변 시퀀스의 패딩 (질문: pre, 답변/타겟: post)
   - 문장 시작(<bos>) 및 끝(<eos>) 토큰 추가

## 모델

- Seq2Seq 모델 구조 (Encoder-Decoder with LSTM)
- Encoder:
   - Embedding 레이어: 토큰 임베딩
   - LSTM 레이어: 입력 시퀀스를 처리하여 문맥 정보를 함축한 최종 상태(hidden, cell state) 출력
- Decoder (Teacher Forcing):
   - Embedding 레이어: 타겟 시퀀스(정답 답변) 토큰 임베딩
   - LSTM 레이어: 인코더 상태를 초기 상태로 받아 임베딩된 타겟 시퀀스를 입력으로 받아 각 시점의 은닉 상태 출력 (Teacher Forcing)
   - Dense 레이어 (softmax 활성화): 각 시점의 은닉 상태를 다음 토큰의 확률 분포로 변환
- 사용된 주요 레이어: Embedding, LSTM, Dense
- 학습 방식: Teacher Forcing

## 학습

- 모델 컴파일 설정:
   - 손실 함수 (Loss): sparse_categorical_crossentropy
   - 옵티마이저 (Optimizer): Adam
- 학습 파라미터:
  - 에폭 (Epochs): 100
   - 배치 크기 (Batch Size): 64
   - 검증 데이터 비율 (Validation Split): 0.2

## 추론

- 추론 모델 역할: 학습된 Seq2Seq 모델을 활용하여 새로운 질문에 대한 답변 생성
- 구조: 인코더 모델로 질문을 처리하여 문맥 상태를 얻고, 추론용 디코더 모델로 답변 토큰을 하나씩 생성
- 디코딩 전략: Greedy Decoding (각 시점에서 가장 확률이 높은 토큰 선택)
- SentencePiece를 사용하여 토큰 ID를 문장으로 변환

## 사용법

- 1. 필요한 라이브러리 설치: pandas, sentencepiece, tensorflow
- 2. 데이터 파일 다운로드 또는 준비: ChatbotData.csv
- 3. SentencePiece 모델 학습 및 모델 파일(.model, .vocab) 생성
- 4. 제공된 Jupyter Notebook (또는 Python 스크립트) 실행
   - 데이터 로딩 및 전처리
   - 모델 생성 및 학습 (decoder_teacher_forcing_model.keras 파일 저장)
   - 추론 모델 로딩 및 사용
- 5. 챗봇 인터페이스를 통해 질문 입력 (예시 제공)
   - '종료' 입력 시 챗봇 종료

