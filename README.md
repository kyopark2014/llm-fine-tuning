# Fine Tuning

## LLM의 Fine tuning 방법

### Instruction Tuning
- Supervised
- 데이터는 소량의 "명령과 입력-출력 쌍"으로 구성됨

### Alignment Tuning
- RLHF, DPO가 해당
- 사람의 피드백을 이용한 튜닝

### Domain-Adaptation
- Self Supervised
- 다량의 "Unlabeled 데이터"사용



Continued pre-training / Instruction Tuning / Alignment Tuning (RLHF, DPO 등)

### QLoRA

[QLoRA](https://github.com/daekeun-ml/genai-ko-LLM/tree/main/fine-tuning)는 
#### Fine-tuning의 어려움

한 예로서 65B 파라미터를 가진 LLaMA 모델을 일반적인 16비트 파인튜닝으로 훈련시키려면 780GB 이상의 GPU 메모리가 필요합니다.

#### 특징

- 4비트로 양자화된 사전 훈련된 언어 모델을 통해 그래디언트를 역전파하여 Low Rank Adapters (LoRA)를 파인튜닝합니다.
- 성능 저하 없이 메모리 사용량을 크게 줄입니다.
  - 단일 48GB GPU에서 65B 파라미터 모델을 파인튜닝이 가능하게 함



