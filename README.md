# 이미지 색상화 및 손실 부분 복원 AI 경진대회
![image](https://github.com/user-attachments/assets/f297bed5-d06d-46d0-a70d-a3dea0b98e6f)
---
## 📌 대회 소개
"이미지 색상화 및 손실 부분 복원 AI 경진대회"는 손상되거나 결손된 이미지를 복구하고, 흑백 이미지를 자연스러운 컬러 이미지로 변환하는 AI 기술을 개발하는 것을 목표로 합니다.
이 기술은 역사적 사진 복원, 영상 편집, 의료 이미지 복구 등 다양한 분야에서 활용 가능성이 높습니다.

이 프로젝트는 흑백 및 손실된 이미지를 복구하여 원본과 최대한 유사하게 만드는 AI 모델을 개발하는 데 중점을 둡니다.

📂 대회 데이터셋 정보
1. 학습 데이터
train_input: 손실된 흑백 이미지 (29603장)
train_gt: 원본 컬러 이미지 (29603장)
train.csv: train_input과 train_gt의 Pair 정보
2. 평가 데이터
test_input: 손실된 흑백 이미지 (100장)
test.csv: 평가용 이미지 경로 정보
3. 제출 형식
결과 이미지를 PNG로 저장 후 zip 파일로 제출
제출 zip 파일은 이미지로만 구성 (폴더 없이)
파일명은 test_input과 동일하게 설정

🏆 대회 목표
손실된 이미지의 결손 부분을 복구하고 흑백 이미지를 자연스러운 컬러 이미지로 변환하는 Vision AI 알고리즘을 개발합니다.

평가 기준:
SSIM(Structural Similarity Index Measure): 이미지 구조의 유사도 평가
색상 Histogram 유사도: 색상 정보의 유사도 평가

🚀 프로젝트 구조
repository/
├── data/
│   ├── train_input/          # 학습용 흑백 이미지 (손실 포함)
│   ├── train_gt/             # 학습용 원본 이미지
│   ├── test_input/           # 평가용 흑백 이미지
│   ├── train.csv             # 학습용 데이터 Pair 정보
│   └── test.csv              # 평가용 데이터 정보
├── notebooks/
│   ├── EDA.ipynb             # 데이터 탐색 및 분석 (EDA)
│   └── model_training.ipynb  # 모델 학습 및 검증 코드
├── src/
│   ├── data_loader.py        # 데이터 로더 스크립트
│   ├── model.py              # AI 모델 정의
│   ├── train.py              # 학습 스크립트
│   └── inference.py          # 추론 스크립트
├── results/
│   ├── predictions/          # 추론 결과 저장
│   ├── submission.zip        # 제출용 파일
│   └── best_checkpoint/      # 학습된 최적의 모델 저장
│       └── best_model.pth    # 최적 성능을 보인 모델 체크포인트
├── README.md                 # 리포지토리 설명 파일
├── requirements.txt          # 의존성 파일
└── LICENSE                   # 라이선스 파일

