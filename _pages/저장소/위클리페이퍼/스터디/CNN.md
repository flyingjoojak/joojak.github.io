---
title: "📃 CNN의 기능"
tags:
    - 데이터
    - 딥러닝
    - 머신러닝
date: "2025-09-19"
thumbnail: "/assets/img/thumbnail/CNN구조.png"
---

# 📃 CNN의 기능
---
## 📚 CNN이 왜 필요한가?
초창기 딥러닝에서는 일반 신경망(MLP, Fully Connected Network)을 사용해 이미지를 처리했습니다. 하지만 이 방식은 너무나 비효율적이었습니다.
* 예를 들어, 28×28 크기의 흑백 이미지는 784개의 픽셀 > 입력층에 노드가 784개가 필요합니다.
* 만약 224×224 RGB 컬러 이미지라면? 무려 150,528개의 노드가 필요하게됩니다.
* 이렇게 되면 연산량 폭발 + 이미지 구조 무시 문제가 생깁니다.

CNN은 이러한 문제를 해결하기 위해 등장했습니다.

## 📚 CNN이 왜 더 효율적일까?
MLP(완전연결 신경망)는 픽셀 하나당 노드를 다 써야 해서 차원=노드 문제가 그대로 발생합니다. CNN은 합성곱(커널) 을 통해 데이터를 지역적으로 훑기 때문에, 입력을 다 펼치지 않고 작은 패치 단위로 특징만 뽑음 따라서 필요한 노드 수가 훨씬 줄고 연산 효율이 좋아짐.

## 📚 CNN의 전체적인 네트워크 구조
![CNN구조](/assets/img/CNN구조.png)
CNN의 구조는 기존의 완전연결계층과는 달게 구성되어 있습니다. 완전연결계층에서는 이전 계층의 모든 뉴런과 결합되어있는 Affine계층으로 구현했지만, CNN은 Convolutional Layer과 Pooling Layer들을 활성화 함수 앞뒤에 배치하여 만들어집니다.
1. 첫번째 Convolutional Layer
![CNN구조](/assets/img/CNN1.png)
우선 우리에게 주어진 입력값은 28x28크기를 가진 이미지입니다. 이 이미지를 대상으로 여러개의 필터(커널)을 사용하여 결과값(feature mapping이라고도 합니다)을 얻습니다. 즉 한개의 28x28 이미지 입력값에 10개의 5x5필터를 사용하여 10의 24x24 matrics, 즉 convolution 결과값을 만들어 냈습니다. 그 후 이렇게 도출해낸 결과값에 Activation function(예를 들면 ReLU function)을 적용합니다. 이렇게 첫번째 Convolutional Layer이 완성되었습니다. 우리는 여기서 한 Convolutional Layer는 Convolution처리와 Activation function으로 구성되어 있다는 것을 알 수 있습니다.
2. 첫번째 Pooling Layer
![CNN구조](/assets/img/CNN2.png)
그 다음은 Pooling Layer입니다. 우선 Pooling이라는 개념이 무엇인지 짚고 넘어가야 합니다.
이 전 단계에서 convolution 과정을 통해 많은 수의 결과값(이미지)들을 생성했습니다. 하지만 위와 같이 한 개의 이미지에서 10개의 이미지 결과값이 도출되어버리면 값이 너무 많아졌다는 것이 문제가 됩니다. 때문에 고안된 방법이 Pooling이라는 과정입니다. Pooling은 각 결과값(feature map)의 dimentionality를 축소해 주는 것을 목적으로 둡니다. 즉 correlation이 낮은 부분을 삭제(?)하여 각 결과값을 크기(dimension)을 줄이는 과정입니다.
![풀링](/assets/img/pooling.png)
Pooling에는 대표적으로 두가지 방법으로 Max pooling과 Average pooling이 있습니다. 위 이미지와 같이 Pool의 크기가 2x2인 경우 2x2크기의 matrix에서 가장 큰 값(max)이나 평균값(average)를 가져와 결과값의 크기를 반으로 줄여주게 됩니다.
자, 이제 다시 본론으로 돌아가 그 위의 전체 네트워크 이미지로 돌아가면 Pooling Layer의 결과값이 열개의 12x12 matrics가 된 것을 확인할 수 있습니다.
3. 두번째 Convolutional Layer
![CNN구조](/assets/img/CNN3.png)
이번 Convolutional layer에서는 텐서 convolution을 적용합니다. 이전의 pooling layer에서 얻어낸 12x12x10 텐서(order-3 tensor)를 대상으로 5x5x10크기의 텐서필터 20개를 사용해 줍니다. 그렇게 되면 각각 8x8크기를 가진 결과값 20개를 얻어낼 수 있습니다.
4. 두번째 Pooling Layer
![CNN구조](/assets/img/CNN4.png)
두번째 Pooling layer입니다. 전과 똑같은 방식으로 Pooling과정을 처리해주면 더 크기가 작아진 20개의 4x4 결과값을 얻습니다.
5. Flatten(Vectorization)
![CNN구조](/assets/img/CNN5.png)
그 후 이 4x4x20의 텐서를 일자 형태의 데이터로 쭉 펼쳐준다고 생각해 봅시다. 이 과정을 Flatten 또는 Vectorization 이라고 합니다. 쉽게 말해 각 세로줄을 일렬로 쭉 세워두는 것입니다. 그러면 이것은 320-dimension을 가진 벡터(vector)형태가 됩니다.
그렇다면 왜 이렇게 1차원 데이터로 변형해도 상관이 없을까요? 이 전에 두번째 pooling layer에서 얻어낸 4x4크기의 이미지들은 이미지 자체라기 보다는 입력된 이미지에서 얻어온 특이점 데이터가 됩니다. 즉 1차원의 벡터 데이터로 변형시켜주어도 무관한 상태가 된다는 의미입니다.
6. Fully-Connected Layers(Dense Layers)
![CNN구조](/assets/img/CNN6.png)
이제 마지막으로 하나 혹은 하나 이상의 Fully-Connected Layer를 적용시키고 마지막에 Softmax activation function을 적용해주면 드디어 최종 결과물을 출력하게 됩니다.