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

---

### 🚀 프로젝트 구조

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

---

## 데이터 전처리 설명
preprocessing.py: 데이터 전처리를 수행하는 메인 스크립트입니다.

### 🛠️ 주요 기능
이미지 특징 추출

CLIPProcessor 및 CLIPModel을 사용하여 고차원 이미지 특징 벡터를 생성합니다.
특징 벡터 정규화

normalize 함수를 이용하여 벡터를 정규화(L2 Norm)합니다.
차원 축소 및 시각화

UMAP을 활용하여 특징 벡터를 2차원으로 축소.
결과를 산점도로 시각화합니다.
클러스터링

HDBSCAN 알고리즘을 이용하여 이미지를 클러스터링하고 노이즈를 제거합니다.
결과 저장

학습 데이터와 평가 데이터에 대한 CSV 파일을 생성합니다:
train_preproc.csv: 학습 데이터 정보
test_preproc.csv: 평가 데이터 정보

### 📊 결과 활용
이 전처리 과정은 주어진 데이터가 같은 장소에서 촬영된 사진들이 많은 점을 고려하여 설계되었습니다. 데이터의 특성을 최대한 활용하기 위해 특징 추출, 차원 축소, 클러스터링 등의 기법을 채택하였습니다.

---
## 학습 코드 설명
### 🛠️ 주요 기능 및 사용 이유
##### 1. 데이터 처리
주요 기능:
학습 데이터의 불완전한 영역을 마스킹하고, 추가적인 데이터 증강을 수행하여 손실된 이미지를 생성.
K-Fold 교차 검증을 통해 데이터셋을 학습 및 검증 세트로 분할.

사용 이유:
동일한 장소에서 촬영된 데이터셋의 특성을 고려하여 불완전 영역을 정의하고 이를 학습 데이터로 활용.
교차 검증을 통해 데이터셋의 일반화를 강화하고 과적합을 방지.

##### 2. 모델 정의
주요 기능:
Unet 모델 두 개를 사용하여 흑백 이미지를 복원한 뒤, 색상화하는 두 단계의 학습 과정을 정의.
첫 번째 모델: 손실된 흑백 이미지 복원.
두 번째 모델: 복원된 흑백 이미지를 컬러로 변환.
사용 이유:
Unet 아키텍처는 세그멘테이션 및 복원 작업에 널리 사용되며, 픽셀 단위의 정확한 복구에 적합.
두 단계를 나누어 작업함으로써 각각의 복원 및 색상화 작업의 성능을 최적화.

##### 3. 손실 함수 및 평가 지표
주요 기능:
손실 함수:
L1 및 MSE 손실을 조합하여 픽셀 단위의 복원 성능을 평가.
평가 지표:
SSIM(Structural Similarity Index Measure): 복원된 이미지와 원본 이미지의 구조적 유사성을 측정.
Masked SSIM: 손실 영역에 한정된 SSIM 점수.
히스토그램 유사도: 복원된 이미지의 색상 정보가 원본과 얼마나 유사한지 평가.
사용 이유:
평가 지표를 통합하여 종합적인 모델 성능을 모니터링.
