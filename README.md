# Pet-Eye-Diagnosis-Model

## ⚙️ 개발 환경

운영체제: Window10

프로세서: Intel(R) Core(TM) i5-10400 CPU @ 2.90GHz 2.90GHz

그래픽카드: NVIDIA® GPU RTX 2060S 8GB

RAM: 16GB

Version: Tensorflow-2.10.0 Tensorflow_gpu-2.10.0 cuda 11.2 cuDDN 8.1 python 3.10.6

Code Editor: JupyterNoteBook

## 🔧 딥러닝 모델

### 안구 질환 목록
- 안검염(Blepharitis):  눈을 둘러싸고 있는 두 개의 눈꺼풀 사이에 있는 점막 주변에 생긴
- 안검종양(Eyelid Tumor): 눈을 둘러싸고 있는 두 개의 눈꺼풀 사이에 있는 점막 주변에 생긴 종양
- 안검내반증(Entropion): 안구 주변에 있는 안검의 내 측면이 눈으로 밀려들어 가거나 안구를 압박하는
- 유루증(Epiphora): 눈 주변의 털이 지속적으로 축축해지고 붉은색으로 변하는 증상
- 결막염(Conjunctivitis): 눈 결막에 생기는 염증
- 핵경화(Nuclear Sclerosis): 수정체의 중심부가 굳어지는 증상
- 백내장(Cataract): 무색 투명한 조직인 수정체에 혼탁이 생기는 증상
- 색소침착성 각막염(Pigmented Keratitis): 각막에 색소가 침착되어 염증이 동반되는
- 궤양성 각막질환(Corneal Ulcerative Disease): 각막의 표면이 손상되어 열공이 생기는 상태로 눈이 붉어지고 부어오름
- 비 궤양성 각막질환(None Corneal Ulcerative Disease): 각막 아래층까지 염색되지 않는 각막질환
- 무증상(NoneExistence): 증상이 발견되지 않은 상태

### 참고 자료
- 해당 블로그에 업로드 된 코드를 참고하여 구현하였습니다.](https://zeuskwon-ds.tistory.com/49)

### 학습 데이터 출처

- [AI HUB 반려동물 안구 질환 데이터](https://www.aihub.or.kr/aihubdata/data/view.do?currMenu=&topMenu=&aihubDataSe=realm&dataSetSn=562)

최종 모델의 학습 DataSet 은 " 141,452장 - train DataSet ", " 35,363장 - 검증 DataSet ", " 22,058장 - test DataSet "으로 구성하였습니다.
</br>


## ⛏️모델 테스트
더 많은 데이터를 넣어 시도해본 전이학습 1epoch 시간과 accuarcy입니다.

아래 표의 데이터 셋의 비율은 간이안구진단모델_테스트.ipynb에서 확인할 수 있습니다.

v1과 테스트 파일의 데이터 셋의 차이가 있습니다. 그리고 v1에는 overfitting을 방지하는 callbacks 옵션을 추가하였습니다.

![1](https://user-images.githubusercontent.com/109027302/230709038-16776a4c-fa41-46ff-a25a-55e08a28b741.PNG)

표를 통해 accuracy는 “DenseNet201”모델이 가장 높음을 알 수 있었고, 이를 메인 전이 학습모델로 설정하고 출력층을 라벨의 개수만큼 조절하여 진행하였습니다.

그러나 early stopping 을 설정하고 epoch를 높였을 때, validation loss가 더 이상 내려가지 않았는 과적합 현상을 발견할 수 있었고, accuracy 또한 만족스러운 결과를 내지 못해 새로운 CNN 구조를 직접 작성하여 모델을 학습시켰습니다.

개선한 CNN 구조를 통해 accuracy를 높일 수는 있었으나, 학습시킨 모델을 테스트한 결과 새로운 데이터에 대한 정확도가 기준치 더 낮았기 때문에, 예측 결과 상위 3개를 보여주는 방 향으로 설정하였습니다.

## 🚫 해결했던 문제!

## 최종 모델 정확도
<img width="787" alt="스크린샷 2023-09-02 오후 1 13 21" src="https://github.com/Pushedsu/Pet-Eye-Diagnosis-Model/assets/109027302/59e833cb-433b-4001-88fb-8e5a6b3e2295">

Test DataSet으로 정확도를 구해본 결과 61.87퍼센트의 정확도를 나타내었습니다.

다른 모델을 적용하여도 정확도는 비슷하였기에 정확도를 높이는 방안으로는 더 많은 데이터 추가 혹은 클래스의 수를 줄여야 했습니다.

데이터를 추가하기에는 데이터가 없었고 클래스를 줄이자니 진단할 질 병들의 수가 확 줄어드는 문제가 있었습니다.

초기 안구 진단 서비스 방식은 단일 예측 값만 출력하였기에 정확률이 크지 않았습니다.

따라서 사용자에게 제공할 예측 값들을 추가하여 부정확한 예측을 보완하기 위해 상위 3개 예측 결과를 보여주어 진단의 정확율을 높여 보조 안구 진단 서비스를 구현하는 방식으로 바꾸었습니다.

<img width="770" alt="스크린샷 2023-09-02 오후 1 10 43" src="https://github.com/Pushedsu/Pet-Eye-Diagnosis-Model/assets/109027302/ca273af3-243b-4644-9068-1bfa19e3212f">

top3.ipynb 파일에 코드를 살펴보시면 상위 3개 진단 결과 중에 진단 결과가 참일 확률이 84.36%로 이전보다 사용자에게 제공하는 정보의 정확율가 높아짐 을 확인할 수 있습니다.

## 서비스 구현 시 사용된 모델
- [서비스에 사용된 모델.ipynb](https://github.com/Pushedsu/Pet-Eye-Diagnosis-Model/blob/main/%EA%B0%84%EC%9D%B4%EC%95%88%EA%B5%AC%EC%A7%84%EB%8B%A8_v3.ipynb)
