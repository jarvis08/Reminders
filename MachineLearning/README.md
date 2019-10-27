### 목차

<br>

<a href="https://github.com/jarvis08/Reminders">메인으로</a>

<br>

### Unsupervised Learning

예시: K-means Algorithm, PCA, SVD

<br>

### Active Learning

Supervised Learning을 통해 학습한 후, 모델 강화를 위해 새로운 데이터를 추가하는 과정에서 적용합니다.

현실 속의 데이터는 정제, 태깅 등의 작업이 되어 있지 않습니다. 따라서 이러한 작업들을 자동화 하는 것에 대한 활발한 연구가 이루어 지고 있습니다.

자동 태깅의 방법의 예시로, 아래 두 가지가 있습니다.

- 데이터를 모델에 적용하며, Softmax의 확률값이 label 별로 고르게 분포된 경우, 연구 및 개발자의 직접적인 태깅이 필요하다고 여긴다. 하나의 값이 높다면, 해당 label로 자동적으로 설정할 수 있습니다.
- k-means와 같이, 데이터 분포의 대표값으로 설정합니다.