#### 사용한 모델은 총 3가지 <img src="https://pic.sopili.net/pub/emoji/twitter/2/72x72/1f525.png" width=20 height=20>

- [1. chatbot을 위한 다중분류 모델](#1-chatbot을-위한-다중분류-모델)

- [2. 인식되는 음성의 보이스피싱 여부를 판단하는 모델](#2-인식되는-음성의-보이스피싱-여부를-판단하는-모델)

- [3. 인식되는 음성의 감정 상태를 판단하는 모델](#3-인식되는-음성의-감정-상태를-판단하는-모델)

- 위의 3가지 모델은 Model 폴더에서 접근 가능

---

## 1. chatbot을 위한 다중분류 모델

- 인식된 음성을 Web Speech API를 이용해서 Text로 변환하고 해당 Text의 Target을 판단하기 위한 분류 모델

#### 학습을 위한 Dataset

![image](https://user-images.githubusercontent.com/26592315/165872101-cbbf32fb-2998-4544-88c2-ab41c84d4644.png)

- 현재까지 2000개 가량의 데이터 셋 확보

#### label 목록

![image](https://user-images.githubusercontent.com/26592315/165872133-148d2f91-bcc0-478a-8d63-3f5a98aacae2.png)

- 주요 금융 업무에 대한 라벨링 작업

#### 모델 설명

- Text를 워드 임베딩하고 BoW를 이용한 TfidfVectozier 기법을 적용하고 MLPClassifier로 학습을 진행

- 학습된 모델로 입력된 Text에 대한 label 값 예측

- 정확도 96%

---

## 2. 인식되는 음성의 보이스피싱 여부를 판단하는 모델

#### 학습을 위한 Dataset

- [(AI 허브) 위급상황 음성/음향 소개 데이터 셋](https://aihub.or.kr/aidata/30742)

#### 모델 설명

음성 데이터를 통한 보이스피싱 탐지 모델

음성데이터를 학습함에 있어서 가장 중요한 점은 보통 접하는 음성데이터는 음압을 측정한 수치이기 때문에 그 자체로 분석하기에는 적합하지 않습니다.

![image](https://user-images.githubusercontent.com/26592315/170874532-4b844905-4402-43e3-bd86-ee9c8ac95bc5.png)

- 음성 원천 데이터

따라서 여러 논문을 탐독한 결과 위험상황 음향 감지 시스템에서는 MFCC(Mel- Frequency Cepstral Coefficient)가 적절한 음성 Feature값임을 찾아냈습니다.

![image](https://user-images.githubusercontent.com/26592315/170874544-e842bf42-aa7a-4e6c-9b2f-2233c191e556.png)

- 음성 데이터에서 뽑아낸 MFCC 값

그렇기에 음성데이터에서 MFCC Feature 값을 추출한 후 Feature 값에 따른 상황 분류를 학습을 했습니다.

    - MFCC의 경우는 파라미터의 값에 영향을 많이 받으며 자연어 처리 분야에서는 40분의 1초의 크기를 기본으로 하고 있어서 sr의 40분의 1로 파라미터의 값을 주었습니다.

    - 또한 MFCC의 n_mels 파라미터의 경우는 mel filter의 개수를 나타내며 성분 수와 비례 관계임으로 128으로 주게 되면 colab에서 RAM이 터지며 40, 60, 80을 비교해보았고 정확도가 가장 높은 40으로 파라미터 값을 정했습니다.

    - MFCC 값을 1차원으로 변경 후 test 음성 데이터의 크기를 맞추기 위해서 평균으로 padding을 주었습니다.

Librosa로 테스트 데이터에 대한 MFCC값을 전부를 뽑아내고 CNN 모델을 사용하여 이진 분류를 진행하였습니다.

정확도는 81%입니다.

학습한 모델은 AI 행원 이용 로직에서 인식되는 모든 음성들을 split하여 서버에 전송하고 서버에서는 음성 데이터를 보이스피싱 모델에 적용하여 예측 값을 뽑아냈습니다.

Predict해서 뽑아낸 값을 이용하여 1, 2, 3단계의 보이스피싱 의심 단계를 설정을 했습니다.

---

## 3. 인식되는 음성의 감정 상태를 판단하는 모델

#### 모델 설명

![image](https://user-images.githubusercontent.com/26592315/170874588-35538994-a3b6-44b6-ad4c-7b636bdbe88c.png)

다음과 같은 두 가지 논문을 기반으로, 본 감정 분석 모델 역시 이전의 보이스피싱 감지 모델과 같이 음성의 MFCC feature값을 추출하여 특징을 학습시키는 방식으로 구현하였습니다.

다양한 학습용 음성 데이터들과 LSTM, ResNet과 같은 모델들의 조합 중에서 VGG16 모델에 TESS데이터를 학습시키는 것이 가장 정확도가 높게 나옴을 직접 확인하였고 이를 감정 분류 모델 구현에 적용하였습니다.

TESS 데이터는 총 2800개의 대상어 집합으로 구성되어 있고,
7가지의 감정을 VGG16모델을 이용하여 다중분류 합니다.
