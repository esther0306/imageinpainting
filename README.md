# 🌈 이미지 색상화 및 손실 부분 복원 AI 경진대회
![image](https://github.com/user-attachments/assets/f297bed5-d06d-46d0-a70d-a3dea0b98e6f)
---
## 📌 대회 소개
> **"손실된 이미지를 복구하고 흑백 이미지를 컬러화하는 AI 알고리즘 개발!"**  
이 기술은 역사적 사진 복원, 영상 편집, 의료 이미지 복구 등 다양한 분야에서 활용됩니다.
"이미지 색상화 및 손실 부분 복원 AI 경진대회"는 손상되거나 결손된 이미지를 복구하고, 흑백 이미지를 자연스러운 컬러 이미지로 변환하는 AI 기술을 개발하는 것을 목표로 합니다.

이 프로젝트는 흑백 및 손실된 이미지를 복구하여 원본과 최대한 유사하게 만드는 AI 모델을 개발하는 데 중점을 둡니다.

---

### 📂 **데이터셋 정보**
| 데이터 유형    | 설명                                        |
|----------------|-------------------------------------------|
| `train_input`  | 손실된 흑백 이미지 (29603장)               |
| `train_gt`     | 원본 컬러 이미지 (29603장)                |
| `train.csv`    | `train_input`과 `train_gt`의 Pair 정보     |
| `test_input`   | 손실된 흑백 이미지 (100장)                |
| `test.csv`     | 평가용 이미지 경로 정보                   |

### 🏆 **대회 목표**
1. 손실된 이미지의 특정 영역 복구  
2. 흑백 이미지를 자연스러운 컬러 이미지로 변환  

### 📝 **평가 기준**
- **SSIM**: 이미지 구조의 유사도
- **색상 히스토그램 유사도**: 색상 정보의 유사도
---

## 🚀 **프로젝트 구조**
```plaintext
repository/
├── data/                   # 데이터 디렉토리
│   ├── train_input/        # 학습용 흑백 이미지 (손실 포함)
│   ├── train_gt/           # 학습용 원본 이미지
│   ├── test_input/         # 평가용 흑백 이미지
│   ├── train.csv           # 학습용 데이터 Pair 정보
│   └── test.csv            # 평가용 데이터 정보
├── notebooks/              # 노트북 디렉토리
│   ├── EDA.ipynb           # 데이터 탐색 및 분석 (EDA)
│   └── model_training.ipynb # 모델 학습 및 검증 코드
├── src/                    # 소스 코드 디렉토리
│   ├── data_loader.py      # 데이터 로더
│   ├── model.py            # 모델 정의
│   ├── train.py            # 학습 스크립트
│   └── inference.py        # 추론 스크립트
├── results/                # 결과 디렉토리
│   ├── predictions/        # 추론 결과 저장
│   ├── submission.zip      # 제출용 파일
│   └── best_checkpoint/    # 최적 모델 저장
│       └── best_model.pth  # 최적 모델 파일
├── README.md               # 리포지토리 설명 파일
├── requirements.txt        # 의존성 파일
└── LICENSE                 # 라이선스 파일
```
---

---

## 데이터 전처리 설명

### 🛠️ **주요 기능**
1. **이미지 특징 추출**  
   - `CLIPProcessor` 및 `CLIPModel`을 사용하여 고차원 이미지 특징 벡터 생성
2. **특징 벡터 정규화**  
   - 벡터를 정규화하여 데이터 간 균일성을 보장
3. **차원 축소 및 시각화**  
   - **UMAP**을 활용하여 2차원으로 축소 후 시각화
4. **클러스터링**  
   - **HDBSCAN** 알고리즘을 사용하여 클러스터링 및 노이즈 제거
5. **결과 저장**  
   - `train_preproc.csv`와 `test_preproc.csv`로 저장

### 📊 **결과 활용**
- 같은 장소에서 촬영된 사진 묶음의 특징을 효과적으로 추출하고, 데이터의 구조적 관계를 시각화.
- 전처리 결과를 기반으로 학습에 적합한 데이터셋 구축.

---
## 학습 코드 설명

### 🛠️ **주요 기능 및 사용 이유**
#### **1. 데이터 처리**
| 기능                          | 설명                                                                                 |
|-------------------------------|--------------------------------------------------------------------------------------|
| 손실 영역 마스킹               | 학습 데이터의 손실된 영역을 생성하여 복원 학습에 활용                                |
| 데이터 증강                   | 랜덤 폴리곤 기반 손실 패턴 생성 및 변환 적용                                         |
| K-Fold 교차 검증               | 데이터셋을 학습 및 검증 세트로 나누어 일반화 성능을 강화                              |

사용 이유:
동일한 장소에서 촬영된 데이터셋의 특성을 고려하여 불완전 영역을 정의하고 이를 학습 데이터로 활용.
교차 검증을 통해 데이터셋의 일반화를 강화하고 과적합을 방지.

#### **2. 모델 정의**
| 기능                          | 설명                                                                                 |
|-------------------------------|--------------------------------------------------------------------------------------|
| 흑백 복원 모델 (U-Net)         | 손실된 흑백 이미지를 복원                                                          |
| 컬러화 모델 (U-Net)            | 복원된 흑백 이미지를 자연스러운 컬러 이미지로 변환                                  |

사용 이유:
Unet 아키텍처는 세그멘테이션 및 복원 작업에 널리 사용되며, 픽셀 단위의 정확한 복구에 적합.
두 단계를 나누어 작업함으로써 각각의 복원 및 색상화 작업의 성능을 최적화.

#### **3. 손실 함수 및 평가 지표**
| 손실 함수                    | 설명                                                                                 |
|------------------------------|--------------------------------------------------------------------------------------|
| L1 및 MSE 손실               | 픽셀 단위 복원 손실을 측정                                                         |

| 평가 지표                    | 설명                                                                                 |
|------------------------------|--------------------------------------------------------------------------------------|
| SSIM                         | 복원된 이미지와 원본의 구조적 유사성                                               |
| Masked SSIM                  | 손실 영역에 대한 SSIM                                                              |
| 히스토그램 유사도             | 색상 정보의 유사도                                                                 |

사용 이유:
평가 지표를 통합하여 종합적인 모델 성능을 모니터링.

#### **4. 학습 및 검증 과정**
| 기능                          | 설명                                                                                 |
|-------------------------------|--------------------------------------------------------------------------------------|
| PyTorch Lightning 사용        | 학습 루프를 간소화하고 가독성을 높임                                                |
| Early Stopping 및 Checkpoint  | 학습 안정성 확보 및 최적 모델 저장                                                  |

사용 이유:
PyTorch Lightning은 학습 코드의 가독성을 높이고, 복잡한 학습 루프를 간소화.
Early Stopping으로 불필요한 과적합 방지 및 학습 효율성을 높임.

#### **5. 모델 예측 및 결과 저장**
| 기능                          | 설명                                                                                 |
|-------------------------------|--------------------------------------------------------------------------------------|
| PNG 이미지 저장               | 테스트 데이터에 대한 복원 결과를 PNG 파일로 저장                                     |
| ZIP 파일 압축                 | 제출 요구 사항에 맞춰 결과를 ZIP 파일로 압축                                         |

---

## 모델 설명: U-Net과 ResNet-34
### 1. U-Net 모델
모델 구조:
U-Net은 **인코더(Encoder)**와 **디코더(Decoder)**로 구성된 대칭형 구조를 가지고 있습니다.

인코더: 입력 이미지를 압축하여 고차원 특징을 추출.

디코더: 인코더에서 추출된 특징을 기반으로 원래 해상도의 이미지를 복원.
픽셀 단위 복원:
U-Net은 픽셀 단위의 복원 작업에 최적화되어 있으며, 의료 영상 및 이미지 복원 분야에서 널리 사용됩니다.

스킵 연결(Skip Connections):
인코더의 중간 출력을 디코더에 직접 연결하여, 원본 이미지의 세부 정보를 복구하는 데 유리합니다.

손실 영역 복원:
손실된 이미지의 특정 영역 복구 및 전체 이미지 색상화와 같은 작업에 적합합니다.

### 2. ResNet-34 백본
모델 구조:
ResNet-34는 Residual Neural Network 계열의 모델로, 깊은 신경망에서도 성능 저하 없이 학습할 수 있도록 설계되었습니다.
Residual Block: 입력을 출력으로 직접 연결하는 스킵 연결이 포함되어 있어, 기울기 소실 문제를 완화.

효율적인 특징 추출:
ResNet-34는 상대적으로 가벼운 네트워크이기에 빠른 학습과 강력한 특징 추출 성능을 제공합니다.

사전 학습된 가중치:
ImageNet 데이터셋으로 사전 학습된 가중치를 사용하여 학습 속도를 가속화하고 초기 성능을 높였습니다.

U-Net의 인코더 강화:
ResNet-34를 U-Net의 인코더로 사용함으로써 입력 이미지에서 더 깊고 의미 있는 특징을 추출할 수 있습니다.

### 3. 모델 구성
첫 번째 U-Net (흑백 이미지 복원):

입력: 손실된 흑백 이미지.
출력: 복원된 흑백 이미지.
구조: ResNet-34 백본을 사용하여 인코더의 특징 추출 성능 강화.

두 번째 U-Net (컬러화):

입력: 복원된 흑백 이미지.
출력: 컬러화된 이미지.
구조: 동일한 ResNet-34 백본을 사용.


### 4. 사용한 PyTorch 라이브러리
Segmentation Models PyTorch (SMP)
SMP는 사전 학습된 백본과 다양한 세그멘테이션 아키텍처(U-Net, FPN 등)를 쉽게 구현할 수 있도록 도와줍니다.
U-Net과 ResNet-34 백본을 빠르게 통합하여 학습에 활용.

###### 📂 SMP 구조 다이어그램
┌──────────────┐
│ Pre-trained  │
│ Backbone     │◀── ResNet-34 (ImageNet)
└──────┬───────┘
       │
┌──────┴───────┐
│  Encoder     │
│  (Extract    │
│  Features)   │
└──────┬───────┘
       │
┌──────┴───────┐
│  Decoder     │
│  (Rebuild    │
│  Image)      │
└──────────────┘
