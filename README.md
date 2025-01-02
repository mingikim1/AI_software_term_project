# AI_software_term_project
본 repository는 충남대 컴퓨터공학과의 AI 소프트웨어 텀프로젝트를 정리하기 위해 만들어졌습니다. Rag 기술을 활용해 AI 지식과 관련된 Q/A 시스템을 개발하는 과제입니다.
본 실습은 무료 Colab 환경에서 진행되었습니다. \
사용된 모델은 다음과 같습니다.

| 항목          | 모델명 | 
|---------------|-------|
| LLM       | [meta-llama/Meta-Llama-3-8B-Instruct](https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct) |
| Embedding        |  [intfloat/multilingual-e5-small-instruct](https://huggingface.co/intfloat/multilingual-e5-small)|

LLM 모델은 Meta Llama의 8B Instruct model을 사용했습니다. 무료 Colab 환경에서 최대한 tight한 모델을 사용하기 위해 노력했습니다.

<img src="https://github.com/user-attachments/assets/7cb4ec47-b949-4937-9096-33146872908f" alt="image" width="400">

 모델을 load하는 과정에서 2가지 아이디어가 충돌했습니다. \
 1. 4bit or 8bit 양자화 후 LoRa를 활용한 fine-tuning
 2. float16, bfloat16으로 모델을 load하는 방식 (fine-tuning X)

1번 방식은 제가 만든 Dataset을 부어 모델을 fine-tuning하고 적은 GPU를 사용하여 만들어낼 수 있다는 장점이 있지만, 반대로 모델 자체의 성능이 떨어지기 때문에 일반화된 능력을 뽐내기 어려웠습니다.
때문에 저는 2번 방식을 선택했고 추후 Prompt에서 Instruct를 강하게 넣어주는 방식을 채택하였습니다. bfloat16으로 모델을 load한 결과 GPU 사용량은 12.2GB를 차지하였습니다. :)

<img src="https://github.com/user-attachments/assets/95cf04c8-49b1-45a5-8314-067cdce3205b" alt="image" width="400"> 

\
다음으로 생각할 것은 PDF 전처리입니다.
우선 교수님께 받은 PDF파일이 서로 다른 주제들이 합쳐져 있는 것을 확인했습니다. 직접 hard하게 주제별로 PDF를 쪼개주었고 총 51개의 PDF를 얻었습니다.
\
이후 정규표현식을 활용해 문단별로 쪼개주고, 5개의 문단을 합쳐준 후 2개를 overlap 해주었습니다.

<img src="https://github.com/user-attachments/assets/5b6cb3ad-e8f4-445d-ab68-b93a52c36f6e" alt="image" width="500"> 

\
실제로 구분된 문단들을 살펴보면 다음과 같습니다!

<img src="https://github.com/user-attachments/assets/694f39ac-8bc5-48a0-8f7a-be4ded917d67" alt="image" width="500"> 

꽤나 문단별로 잘나누어 진 것을 확인할 수 있었습니다 :)
\
Prompt는 AI 지식을 말하긴하되 너무 PDF에 치중되지 않은 답을 달라고 했습니다. 양자화를 하지 않았기 때문에 좀 더 일반화된 나은 성능을 보일 수 있을 것입니다.

<img src="https://github.com/user-attachments/assets/1a0cf0f1-76a1-4820-9300-58d738cc161c" alt="image" width="500"> 


\
마지막으로 Chat Interface를 확인해보면 다음과 같습니다.

<img src="https://github.com/user-attachments/assets/fb67e638-91a8-4a57-a5db-d5d470ec6814" alt="image" width="500"> 

fastapi를 통해 Chat Interface를 만들어보았습니다! 간단한 질문으로 transformer가 무엇입니까? 라고 질문했을 때 잘 대답하는 것을 확인할 수 있었습니다.

[발표영상](https://youtu.be/t8L2mYovrBE)
