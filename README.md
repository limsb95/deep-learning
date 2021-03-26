# deep-learning

딥러닝 이미지 처리 핵심 알고리즘 분석과 활용

1. CNN 아키텍처와 핵심 기술의 이해

FNN
- FNN은 Input을 1차원화 한뒤, 통과 시킨다
- 학습이 완료되면, 어떤 가중치가 벡터를 추출하는데에 적합한지 그리고 어떤 벡터가 중요한 값인지 알게된다
- 특정 추출된 입력 이미지의 벡터들이 연산 계층을 통해 합쳐져서 입력 데이터의 특징들이 추출된다
- 이런 특징들이 계층들을 지나면서 서로 연관되며 특징들간의 패턴이 추출된다

FNN 한계점
- FNN은 완전연결 신경망이다. 입력 데이터가 1차원으로 바뀌어야 완전연결이 가능하며 이 과정에서 입력 데이터의 형상이 무시된다
- 입력 데이터가 이미지일 경우, 다양한 공간적 정보와 인접 픽셀간 높은 상관관계가 존재하기 때문에 이미지를 벡처화 하는 과정에서 큰 정보 손실이 발생한다
- 입력 데이터가 변경되면 같은 이미지임에도 1차원화를 하면 전혀 다른 벡터가 나오므로 위치나 크기에 매우 민감하다

CNN
- 이미지 자체에서 특징을 추출하고 그 특징들간의 패턴을 알아낸 뒤 FNN을 통해 학습하는 방식이 CNN
- 이미지의 일부만 반응시키는 과정은 필터링 기술을 써서 분리 시킨 뒤 특징추출 (특징맵)
- 행렬로 표현된 필터의 요소들이 문체의 고유 특성을 파악할 수 있게 필터를 학습시키는게 목표이다

합성곱 연산 Convulution 
- 입력 데이터에 필터(커널)을 씌워 윈도우를 이동시켜가며 대응되는 원소끼리 곱한 뒤 총합하는 연산
- CNN 에서 필터의 매개변수가 가중치 이다
- 필터를 여러개 써서 여러 특징들을 추출해서 객체의 클래스를 추론하는 정확도를 높인다
- 
3차원 데이터의 합성곱
- 3차원 입력 데이터와 3차원 필터를 합성곱 연산시, 2차원 출력 데이터가 나온다
- 2차원 출력 데이터를 여러개 얻기위해 필터를 여러장 쓰면 3차원 데이터가 된다
- 필터의 개수로 출력의 채널 크기를 조절할 수 있으며 입력 데이터의 채널과 필터의 채널은 같아야 한다

Padding
- Convulution 전에 입력 데이터를 특정 값으로 채우는 과정
- 출력데이터의 크기를 증가한다

Stride
- 필터를 이동하는 정도
- 출력데이터의 크기를 감소한다

합성공 연산에 패딩과 스트라이드 사용시 출력의 크기

floor[(n+2p-f)/s+1] x floor[(n+2p-f)/s+1]

input: n x n
filter: f x f
padding: p
stride: s

Pooling
- 가로 세로 방향의 공간을 샘플링을 통해 줄이는 연산
- 특징에 반응 하는 부분은 많지 않아 제거해줌으로써 연산의 효율이 증가한다
- Max Pooling, Average Pooling 등등이 있다
- 채널마다 독립적으로 계산하므로 채널수는 바뀌지 않는다

Conv, Pool 계층들을 통과시키다가 마지막에 FC(Fully Connected) 계층을 써서 특징맵을 합쳐준다. 
필터1에 의해 생긴 특징맵이 필터2에 반응하며 필터를 지날수록 차원적 관련성 때문에 단순한 특징들이 조합된 복잡한 특징이 만들어진다.
(5x5) 입력 데이터에 (5x5) 필터를 합성곱시 나오는 결과는 (3x3) 필터를 2번 수행하는것과 같은 결과를 얻을 수 있다
이때 (5x5) 필터를 쓰면 매개변수가 25개인것이 (3x3) 필터를 쓰면 18개로 줄어든다
즉, 층을 깊게 하면 더 적은 매개변수로 같은 표현력을 얻을 수 있다

2. Object Localization & Landmark Detection

Classification
- 입력으로 주어진 이미지 안의 객체의 종류를 구분하는 행위

Localization
- 주어진 이미지 안의 객체가 이미지의 어느 위치에 있는지 위치 정보를 출력해주는 것
- Bounding Box 를 많이 사용하며 픽셀 좌표가 아닌 left right top bottom 으로 출력된다

Object Detection
- 객체 검출은 보편적으로 Classification 과 Localization을 동시에 수행되는 것을 의미한다
- 모델 학습 목적에 따라서 특정 객체만 검출하는 경우도 있고 여러개의 객체를 검출하는 경우도 있다
- 이미지 위에 모델이 학습한 객체의 위치만 Bounding Box 로 표현된다

Landmark Detection 
- Bounding Box 를 쓰지않고 여러 개의 점(Landmark)을 찍는 방식이다

Object Segmentation
- OD를 통해 검출된 댇체의 형상을 따라서 영역을 표시하는 것이다
- 각 픽셀을 classification 해서 결과 값을 도출한다
- 단순히 foreground 와 background 를 구분하는 용도로 쓰이기도 한다

3. Object Detection 알고리즘의 이해와 활용
- ConvNets을 통한 Sliding Windows Detection Algorithm (computational cost 단점)
- Bounding Box Predictions
- Intersection Over Union
- Non-max Suppression
- Anchor Boxes
- Region Proposals

R-CNN
- R-CNN은 region proposal이라는 수백개의 이미지 후보를 생성하고 각각에 대해서 분류를 한다
- 후보로 뽑힌 부분들은 Classification network에 의해 분류가 이뤄지고 Bounding Box를 찾는다
- 제안된 영역이 어떤 클래스인지 분류하기 위해서 많은 레이어들을 통과해야만 한다
- Proposal 수도 많고 과정에서의 오버헤드도 크기때문에 느리다

YOLO
- YOLO는 격자 그리드로 나누어 한 번에 클래스를 판단하고 이를 통합해 최종 객체를 구분하며 동영상도 거의 실시간으로 동작할 만큼 빠른 속도를 자랑한다
- 단 하나의 네트워크가 한번에 특징도 추출하고, Bounding Box도 만들고, 클래스고 분류한다
- 경계 박스 안쪽에 어떤 오브젝트가 있을 것 같다고 확신이 들수록 박스는 굵게 그려진다
- 굵은 경계 박스들만 남기고 NMS 알고리즘을 이용해 선별한다
- 그리드 안에 오브젝트가 여러개 있으면 검출을 잘 못한다는 담점이 있지만 이건 YOLOv2 얘기이며 현재는 v4 까지 나오면서 성능은 좋아지고 있다


4. Generative Models 이해와 활용
- 단순하게 값을 예측하는 모형을 넘어서서, 데이터나 자연 현상 자체를 딥러닝으로 모델링 하려는 시도가 많은데 이것을 생성모델이라고 한다
- 실제 현상을 모델링 하기 위해서는 확률의 개념이 필수

GAN
- Generative Adversarial Network
- 두개의 인공신경망을 서로 경쟁을 시켜서 성능을 향상시키는 알고리즘
- 생성자는 실제 데이터와 아주 유사한 데이터를 생성해낸다
- 감별자는 실제 데이터와 생성자가 생성해낸 모조 데이터를 받아서 진짜 데이터와 가짜 데이터를 판별한다
- 생성자와 감별자를 경쟁적으로 학습 시킨다
- 생성자의 목적은 진짜와 유사한 데이터를 만들어 감별자를 속이는 목적
- 감별자는 모조 데이터를 찾아내는것이 목적
