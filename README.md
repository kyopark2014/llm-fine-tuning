# Fine Tuning

## LLM의 Fine tuning 방법

### Instruction Tuning
- Supervised
- 데이터는 소량의 "명령과 입력-출력 쌍"으로 구성됨

### Alignment Tuning
- RLHF, DPO가 해당
- 사람의 피드백 (Reward)을 이용한 튜닝

### Domain-Adaptation
- Self Supervised
- 다량의 "Unlabeled 데이터"사용

Continued pre-training / Instruction Tuning / Alignment Tuning (RLHF, DPO 등)

## LLM Overview

[A Comprehensive Overview of Large Language Models](https://arxiv.org/pdf/2307.06435.pdf)에서는 아래와 같이 LLM의 5가지 Branch인 1) Training 2) Inference 3) Evaluation 4)Applications 5) Challenges를 정리하고 있습니다.

여기서 Fine-tuning은 전체 파라메터를 튜닝(Full Parameters)을 하거나 효과적으로 파라미터를 튜닝(Parameter Efficient)로 나누고 있습니다. 효율적으로 파라미터를 튜닝하기 위해서 사용할 수 있는 방법에는 1) LoRA 2) Adeapter Tuning 3) Perfix Tuning 4) Prompt Tuning 5) LLaMA Adapter를 설명하고 있습니다.

![image](https://github.com/kyopark2014/fine-tuning/assets/52392004/6af4e463-c39b-4311-9e21-c91f12d1cbf8)


### Model의 Parameter Tuning 방법

Parameter Tuning방식에는 아래와 같은 방식들이 있습니다. 

1) Retain All Paramters: 모든 파라미터를 업데이트 함
2) Transfer Learning: 일반적으로 신경망(NN)의 "헤드”(상위 레이어)를 삭제하고 새 것으로 교체하여 업데이트합니다.
3) PEFT (Parameter Efficient Fine-Tuning): 상대적으로 적은 수의 학습 가능한 매개변수를 사용하여 기본 모델을 보강하는 용도로 사용합니다. 대표적으로 LoRA가 있으며, Instruction tuning에 많이 사용됩니다.

Parameter-efficient Fine-tuning을 비교하면 아래와 같습니다. 여기서 PLM은 pre-trained language model입니다.

![image](https://github.com/kyopark2014/fine-tuning/assets/52392004/06a950c2-8821-4b76-b224-63d95f69f4ef)

#### Fine-tuning의 어려움

65B 파라미터를 가진 LLaMA 모델을 일반적인 16비트 파인튜닝으로 훈련시키려면 780GB 이상의 GPU 메모리가 필요합니다.


#### LoRA

Fine-tuning에서 메모리 요구 사항을 줄이기 위한 방법으로 전체 모델 매개변수는 고정된 상태에서 작은 수의 훈련 가능한 매개변수인 어댑터를 사용합니다. 

![image](https://github.com/kyopark2014/fine-tuning/assets/52392004/8153a2ee-f930-435f-82c3-1a0b1c67452c)


- 확률적 경사 하강법(stochastic gradient descent)을 이용하여 Gradient는 고정된 사전 훈련된 모델 가중치를 통과하여 어댑터로 전달됩니다.
- Adapter는 Loss Function을 최적화하도록 업데이트 됩니다.

#### QLoRA (Quantized LoRA)

2023년 5월에 나온 [QLoRA](https://github.com/daekeun-ml/genai-ko-LLM/tree/main/fine-tuning)는 4비트 양자화된 Pre-trained LLM을 이용하여 Gradient를 역전파하여 LoRA(Low Rank Adapters)를 Fine-tuning합니다. 65B LLaMA를 16비트 Fine-tuning을 하라면 780GB의 GPU메모리가 필요한데, QLoRA를 이용하면 거의 성능 저하 없이 48GB GPU에서 Fine-tuning을 할 수 있습니다. 상세한 내용은 [QLoRA: Efficient Finetuning of Quantized LLMs](https://github.com/daekeun-ml/genai-ko-LLM/tree/main/fine-tuning)와 [Parameter Efficient Fine-tuning of LLMs: Maximizing Performance with Minimal Parameter Updates](https://medium.com/@zaiinn440/parameter-efficient-fine-tuning-of-llms-maximizing-performance-with-minimal-parameter-updates-5ff0cb54032)을 참조합니다. 

여기서 양자화(Quatization)란 32bit 부동소수점을 4bit정수로 변환하는것과 같이 입력데이터를 더 적은 정보를 가지도록 변환합니다. 

![image](https://github.com/kyopark2014/fine-tuning/assets/52392004/c640e78b-69c7-494a-954b-51c19a5c6c17)


- LoRA를 개선하기 위하여 4bit 양자화
- 메모리 spike를 방지하기 위하여 [paged optimizer](https://github.com/daekeun-ml/genai-ko-LLM/tree/main/fine-tuning#3-c-paged-optimizers)를 사용합니다. 여기서 Paged Optimizers는 NVIDIA의 통합 메모리 기능을 활용하여 긴 시퀀스 길이의 미니배치를 처리할 때 발생하는 그래디언트 체크포인팅 메모리 스파이크를 피하는 기술입니다.

## Bedrock에서 Fine-Tuning

[AWS Bedrock에서 제공](https://aws.amazon.com/ko/blogs/aws/customize-models-in-amazon-bedrock-with-your-own-data-using-fine-tuning-and-continued-pre-training/)하는 Fine tuning은 방식에는 아래 2가지가 있습니다.

- Fine-Tuning: Instruction Tuning에 해당되고, 소수의 lable된 Sample 데이터를 활용하는 Supervised 형태 입니다. 이것은 특정 작업의 정확도를 높이는데 효과적입니다.
- Continued pre-training: Domain Adaptation에 해당되고 대량의 label되지 않은 데이터 세트를 활용하는 Self-Supervised 형태 입니다. 이것은 도메인에 대한 모델의 정확도를 유지하면서 파인튜닝을 할 수 있습니다. 






