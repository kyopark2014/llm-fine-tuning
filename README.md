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

### QLora

[QLoRA](https://github.com/daekeun-ml/genai-ko-LLM/tree/main/fine-tuning)는 4비트로 양자화된 사전 훈련된 언어 모델을 통해 그래디언트를 역전파하여 Low Rank Adapters (LoRA)를 파인튜닝합니다.



