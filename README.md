## 학습 

1. AI 행원이 음성을 인식 후에 반응을 위한 text 학습.

2. 보이스피싱 감지 기능 ( 목소리만 가지고 판단 )


## 1번 학습을 위한 Dataset

![image](https://user-images.githubusercontent.com/26592315/165872101-cbbf32fb-2998-4544-88c2-ab41c84d4644.png)

- 현재까지 2000개 가량의 데이터 셋 확보

![image](https://user-images.githubusercontent.com/26592315/165872133-148d2f91-bcc0-478a-8d63-3f5a98aacae2.png)

- label 목록


## 1번 모델

- TfidfVectorizer으로 문장을 숫자화 하고  MLPClassifier를 사용해서 다중분류를 처리


## 2번 학습을 위한 Dataset

- [(AI 허브) 위급상황 음성/음향 소개 데이터 셋](https://aihub.or.kr/aidata/30742) 

## 2번 모델

- Audio feature을 잘 선택하고
- Logistic Regression , Support Vector Machine , Decision Tree , MFCC를 이용한 CNN 오디오 분류 중에서 선택할 예정