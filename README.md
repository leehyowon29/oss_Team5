<a name="title"></a>
# 제목 : LLaMA 3 에 대해 알아보자
---
<a name="team"></a>
## 팀원 :
+ 통계학과/2024009762/이효원 (leehyowon29) (팀장)
+ 전자공학부 모바일공학전공/2024007587/홍은지 (zjseltus)
+ 통계학과/2024002789/이한선 (ihanseon)
+ 전자공학부 인공지능전공/2024003120/이가령 (GaRyeong9828)
---
<a name="introduction"></a>
# 1. Introduction 
>![image](https://github.com/user-attachments/assets/ade33c07-f448-42af-8cd3-e17b3dc4fb05)
>[LLaMA 3 Official Github](https://github.com/meta-llama/llama3)

LLaMA 3는 Large Language Model Meta AI의 줄임말로 메타(구. 페이스북)에서 주도하는 오픈소스 언어모델이다.
2023년 2월 24일 LLaMA-1이 처음 공개 되었고, 이후 2023년 7월 18일에 LLaMA-2로 한번의 성능 향상을 이룬 뒤, 기업과 상용 모두에 공개하면서 관심을 끌기 시작한다.
2024년 4월 18일, 현재의 LLaMA-3가 공개되었고, 현재 meta.ai를 통해 영미권 국가들을 우선 대상으로 인스타그램 등 메타의 플랫폼에 챗봇으로 도입되었다.
이후 7월 23일, LLaMA-3.1 버전을 8B(Billion), 70B, 405B로 3가지 모델을 출시하면서, 8개 언어와 도구 사용을 지원하였다.
가장 규모가 큰 405B 버전은 GPT-4o, Claude 3.5 Sonnet과 견줄만한 성능을 갖춘 상태이다.

LLaMA는 엔비디아의 GPGPU 자원을 통한 인프라를 구축했으나, 엔비디아의 생산 차질로 인한 공급 이슈와 높은 가격, 비효율적인 전력 소모를 이유로 메타의 인공지능 가속기를 통한 개발 환경을 만들고 있다.
소프트웨어도 CUDA를 벗어나, 메타의 라이브러리인 PyTorch를 통해 컴파일러, 커널, 스트리밍API, 드라이버까지 독자 생태계 구축을 가속화시키고 있다.

1조 4000억개의 토큰으로 학습되었다.
해당 토큰은 페이스북, 인스타그램 등 메타의 패밀리앱 외에도 웹스크래핑/크롤링, 깃허브 코드, 위키피디아 텍스트, 퍼블릭 도메인 서적, LaTeX 코드로 작성된 논문, 질문질답 텍스트 등으로 학습되었다고 한다.

---
<a name="license"></a>
# 2. LLaMA 3 License
>[LLaMA 3 깃허브 LICENSE 문서](https://github.com/meta-llama/llama3/blob/main/LICENSE)

---

<a name="chapter"></a>
# 3. 목차
><a href="#title">제목</a></br>
><a href="#team">팀원</a>
>1. <a href="#introduction">개요</a>
>2. <a href="#license">LLaMA 3 라이선스</a>
>3. <a href="#chapter">목차</a>
>4. <a href="#function">기능 설명 및 원리</a>
>5. <a href="#similar_LLM">유사한 기능의 LLM</a>
>6. <a href="#future">인공지능의 방향성</a>

---

<a name="function"></a>
# 4. 기능 설명 및 원리
## 다언어 번역 기능

>### The limits of my language means the limits of my world. </br>언어의 한계는 세계의 한계다.
>#### Ludwig Wittgenstein (철학자 비트겐슈타인)
수많은 언어로 정리된 세계 여러 정보들을 사용하여, 수많은 언어의 화자들과 소통하기 위해서는 우리는 언어의 한계를 넘어야만 한다. 사용자의 입장에서는 이런 다언어 번역 기능이 직접적인 세계의 확장 경험으로 이어질 수 있는 만큼, 아주 중요한 기능일 것인데, 어떤 원리에서 작동하고, LLaMA의 다언어 번역 기능은 어떨까?

### 다언어 기능의 원리
NLP의 다언어 기능을 구현하는 방법은 크게 세가지가 있다.
사용 번역기를 이용한 다언어 기능 구현, 해당 언어의 모델 학습, 다국어 모델 사용이 구현하는 방법이다.

#### 1. 번역기 기능을 사용한 다언어 기능
가장 기초적인 방안이자 쉬운 방안은 번역기 기능을 응용하는 것이다.</br>
RNN을 기반으로 하는 현재의 구글 번역기나 파파고는 우리가 느끼기에도 일상생활에서 사용하기에는 충분할 정도의 성능을 보여주기 때문에 사용하기에 충분하다고 생각할 수 있으나, LLM 과 같은 자연어 처리 모델에서는 모델과 화자가 대화하는 상황에서 맥락에 맞는 표현을 사용해야하는데 이런 점에서 번역기 기능이 아주 약한 점을 보여준다. 또한, 번역기를 사용하다보면 가끔 느낄 수 있는 문제인 어휘 집합의 제한성 또한 자연어 처리 모델에게 부정적인 영향을 끼친다. 마지막으로 전문적인 어휘 표현에 있어, RNN 기반 번역기는 그저 이전까지의 패턴을 통해 출력하기 때문에 학습데이터에서 소수를 차지하는 전문적인 어휘 표현을 알아낼 수 없다는 단점이 있다.

#### 2. 해당 언어 모델 학습을 통한 다언어 번역 기능
가장 확실하면서 자연스러운 결과물을 만들 수 있는 방안이다.</br>
인공신경망을 기반으로 하는 NLP는 많은 데이터를 학습하게 되면 결국에는 좋은 결과물을 만들어 낼 수 있다. 이 점을 적극 활용하여, 실제 모국어 화자들로부터 자연어 데이터를 가져오는 것으로 아주 자연스러운 대화용 에이전트를 구성할 수 있다. 하지만 이 방식은 명확한 단점이 존재하는데, 다른 언어로 넘어가는 과정에서 문맥적 이해에 충분히 기여하기 힘들다는 단점이 있다. 또한, 가장 큰 단점은 각각의 언어 모델 데이터의 일관성을 가지게 하기 위해서 발생하는 시간 및 비용적 손실이 아주 크다는 단점이 있다. 다언어 번역을 위해서는 각각의 언어로 서로 대치 될 수 있는 정보가 필요한데, 하나의 정보에 대해서 각각의 언어로 자세히 서술된 데이터를 찾기란 많은 비용이 발생하기 때문이다. 추가적으로 각각의 언어 모델이 각각의 네트워크를 구성하기 때문에 학습 및 사용에 대한 자원 소모량이 보다 증가한다.

#### 3. 다국어 모델을 통한 다언어 기능 구현
자연어 처리 모델에 최적화된 방법이며, 현재 가장 많이 사용되는 방안이다.</br>
우선 다국어 모델이란, 다양한 언어를 이해하고 처리할 수 있도록 설계된 신경망 구조이다.

>[다국어 모델](https://tech.kakaoenterprise.com/99) (관련된 추가적인 정보)

각각의 언어에 대해 서로 다른 네트워크를 사용하는 것이 아닌, 학습 과정에서부터 N개의 언어가 조합되어있는 다대다 데이터 세트를 이용해 모델을 구축한다. 이를 통해서 여러 언어가 통합된 하나의 모델을 구축하며 다양한 언어로 구성된 데이터에 보다 강력한 성능을 드러낼 수 있다. 또한, 단일 데이터 셋을 통한 하나의 '다국어'에 대응하는 언어 모델을 구성하였기 때문에 학습 및 사용에 대한 자원 소모량을 줄일 수 있다. 만약 사전 학습된 다국어 모델에 대해서 새로운 언어를 추가하는 방식도 2번 방식보다 더 간단한데, 2번 방식보다 훨씬 적응 양의 데이터로도 파인 튜닝(세부 조정)을 통해 시도해 볼 수 있기 때문이다. 하지만, 하나의 네트워크로 구성되다보니 만약 학습 데이터가 오염되기 시작한다면 모든 언어에 대한 성능 저하가 이전 두 경우보다 강하게 발생할 수 있으며, 2번 방식에 비해서는 하나의 언어에 대한 자연스러움이나 어휘 다양성이 떨어지는 단점이 있다.

### LLaMA 3.2 에서 다언어
그렇다면 실제 LLaMA 3.2에서는 얼마나 다양한 언어를 사용할 수 있을까?

우선 프롬프트에 가능한 언어를 물어보았다.
>![image](https://github.com/user-attachments/assets/d9246061-0483-42a9-81d6-1b5ba9c6e382)

그리고 각각의 어족에서 주요 언어로 자기소개를 부탁해보았다.
>![image](https://github.com/user-attachments/assets/0b337557-aa59-49b3-aa11-8f2a060a18cb)
>인도-유럽어족(게르만어파, 로망스어군) (영어, 스페인어, 프랑스어, 독일어, 이탈리아어)
</br>

>![image](https://github.com/user-attachments/assets/249a9e9d-40c3-4939-91e1-1ceed1fd14ff)
>슬라브어파(인도-유럽어족) (러시아어(영어로 출력됨), 폴란드어, 체코어, 슬로바니아어)
</br>

>![image](https://github.com/user-attachments/assets/d0a08d98-0ac2-4eee-9d40-66db795ec593)
>러시아어 자기소개
</br>

>![image](https://github.com/user-attachments/assets/37ce997f-8122-4407-8a62-bb9afc4f5eab)
>아시아 언어 (중국어(북경), 일본어, 한국어, 칸나다어(인도) ,타밀어(인도))
</br>

>![image](https://github.com/user-attachments/assets/4282078b-4c43-42bf-b0dc-88a8117f3de2)
>아프리카 언어 (요루바어(나이지리아), 암하라어(에티오피아), 아랍어, 하우사어(나이지리아), 줄루어(남아프리카))
</br>

우선 메타에서 공식적으로 밝힌 LLaMA 3.2가 지원하는 언어는 영어, 스페인어, 프랑스어, 독일어, 이탈리아어, 포르투갈어, 네덜란드어, 러시아어로 8개가 가능하며, 3.2버전으로 올라오면서 영어, 스페인어, 프랑스어, 독일어, 이탈리아어, 포르투갈어, 힌디어, 태국어에 대한 개선을 진행했다고 한다. 실제 한국어 및 일본어, 중국어로 테스트를 헀을 때는 좋은 결과를 얻지 못헀으며, 아시아 언어를 출력하는 것 자체가 쉽지 않았다. 알파벳 및 키릴문자와 아랍어 등의 문자에 대해서는 쉽게 출력이 가능했지만, LLaMA 본인이 러시아어 농담을 만들거나 이해하는 것이 쉽지 않다고 말하는 등, 공식적으로 개선을 밝힌 8개 외의 언어에 있어서는 약한 모습을 보였다.</br>

하지만 메타의 LLaMA 3는 다국어 모델을 사용하기 때문에, 파인 튜닝을 통해 다른 언어에도 대응할 수 있는 개선이 쉽다. Hugging Face를 통해 한국어 파인튜닝을 진행시킨 모델을 통해 파인 튜닝 전후를 비교해보자.

### LLaMA-3-Korean-Bllossom-8B-Q4_K_M-1727889541502:latest
한국어에 파인튜닝 된 모델은 해당 버전을 사용했으며 [이 링크](https://huggingface.co/MLP-KTLim/llama-3-Korean-Bllossom-8B)를 통해 접근할 수 있다.</br>

>![image](https://github.com/user-attachments/assets/99a48b48-d719-4463-a898-9eff1f4fd28e)
>한국어로 자기소개를 부탁한 결과
</br>
기본 LLaMA 3.2 버전보다 기본적인 한국어에서 더 나은 성능 및 활용을 보여 주었다. 우선 한국어의 입력과 출력이 가능하고, 기본적인 처리를 할 수 있다. 이의 배경은 파인 튜닝을 위해 사용한 데이터에서 알 수 있다. 서울과기대 슈퍼컴퓨팅 센터의 지원을 기반으로 한 100GB 이상의 한국어 데이터를 바탕으로 하였기 때문에 기존의 LLaMA 3 대비 강력한 한국어 처리 성능을 만들 수 있었다. 

---

## 이야기나 시, 노래를 창작해주는 기능
Llama3는 사용자의 요청에 따라 다양한 형식의 이야기, 시, 노래를 창작할 수 있는 강력한 기능을 제공한다. 이 기능은 창의적인 창작활동을 지원하여 사용자가 원하는 주제나 스타일에 맞춘 독창적인 콘텐츠를 생성한다. 이러한 창의적인 컨텐츠를 사용자는 다양한 분야에서 능동적으로 사용할 수 있다.

+ 사용자 맞춤형 창작

  + **주제 선택**: 사용자는 특정 주제를 선택하여 그에 맞는 이야기, 시, 노래를 요청할 수 있다.
  + **스타일 지정**: 사용자는 고전, 현대, 판타지, 공포 등 다양한 스타일을 지정하여 원하는 분위기와 느낌을 전달할 수 있다.


+ 다양한 형식 지원

  + **이야기**: 짧은 이야기, 소설의 한 챕터 등 다양한 길이의 이야기를 생성할 수 있다.
  + **시**: 자유시, 고전시 형식 등 다양한 시 형식을 지원한다.
  + **노래**: 가사 형태로 노래를 창작하며, 사용자가 요구하는 특정 장르나 주제에 맞춘 곡을 생성한다.


+ 창작 과정

  + **아이디어 생성**: 사용자의 요청에 따라 아이디어를 브레인스토밍하여 초안을 작성한다.
  + **내용 개발**: 초기 아이디어를 바탕으로 내용을 확장하고 다듬어 완성된 작품을 생성한다.


+ 반복 수정 가능

  + **피드백 반영**: 사용자가 제공하는 피드백을 바탕으로 내용을 수정하거나 보완할 수 있다.
  + **버전 관리**: 이전 버전과의 비교를 통해 필요한 수정 사항을 쉽게 찾을 수 있다.


+ 사용 예시

  + **이야기 요청**: "한 여름밤의 꿈 같은 이야기 만들어줘."
   ![oss_소설_한여름](https://github.com/user-attachments/assets/073d0cf8-5c4f-4658-9190-109c874977f6)

  + **시 요청**: "사랑에 관한 아름다운 시를 써줘."
![oss_시_사랑](https://github.com/user-attachments/assets/480a0f6c-b555-4286-a865-db122a480b51)

  + **노래 요청**: "희망에 관한 감동적인 노래 가사를 만들어줘."
![oss_가사_희망](https://github.com/user-attachments/assets/f2614723-4d3a-497d-9368-47643b869464)


+ 활용 분야

  + **교육**: 학생들이 자신이 가진 창의력을 발휘할 수 있도록 도와주는 도구로 사용될 수 있다.
  + **작가 지원**: 작가들이 영감을 얻고 새로운 아이디어를 모색하는 데 유용하다.
  + **취미**: 개인적인 취미로 글쓰기를 즐기는 사람들에게도 큰 도움이 될 수 있다.

---

## 코드 작성 및 디버깅 기능
Llama3는 대규모 언어 모델로, 코드 작성 및 디버깅에 유용한 기능들을 제공한다. 이러한 기능은 개발자들이 코드 품질을 향상시켜 생산성을 높이는 데 도움을 준다. 이 모델을 통해 반복적인 작업에서 벗어나 창의적인 문제 해결에 집중할 수 있게 된다.

+ 코드 작성

  + **자동 완성**: 사용자가 입력한 코드의 맥락을 이해하고 다음에 올 코드를 제안한다.
  + **다양한 언어 지원**: 파이썬, 자바스크립트, 자바, C++ 등 여러 프로그래밍 언어를 지원하여 사용자의 요구사항에 맞춘 코드를 작성할 수 있다.
  + **프레임워크 및 라이브러리 이해**: 특정 프레임워크나 라이브러리에 맞는 코드를 생성할 있다.

+ 코드 디버깅
  
  + **문법 오류 탐지**: 작성된 코드에서 문법 오류를 찾아내고,  이를 수정하는 방법을 제안한다.
  + **논리 오류 분석**: 코드의 논리를 분석하여 예상치 못한 결과를 초래할 수 있는 부분을 찾는다.
  + **테스트 케이스 생성**: 코드를 검증하기 위한 테스트 케이스를 자동으로 생성하여 코드의 신뢰성을 높인다.

+ 코드 최적화
  
  + **성능 개선 제안**: 비효율적인 코드 조각을 찾아내고, 성능을 향상시킬 수 있는 대안을 제시한다.
  + **리팩토링**: 코드의 가독성을 높이기 위해 불필요한 부분을 제거하고, 구조를 개선하는 방법을 제안한다.

+ 문서화
  
  + **주석 추가**: 코드에 필요한 주석을 자동으로 추가하여 이해를 돕는다.
  + **API 문서 생성**: 작성된 코드에 대한 API문서를 생성해 다른 개발자들이 쉽게 이해할 수 있도록 한다.

+ 사용 예시
  + 파이썬으로 간단한 계산기 만들어줘.
![oss_code_1](https://github.com/user-attachments/assets/ff5c1090-1bba-4a6f-973c-610ff69c1497)
![oss_cod_2](https://github.com/user-attachments/assets/92db0712-7765-4513-888f-12e0906d7f4f)
![oss_code_3](https://github.com/user-attachments/assets/11011a65-1314-4bdc-a6fb-7dcc42a4bcfa)

+ 주의사항
  
	+ **보안**: 자동생성된 코드가 보안 취약점을 포함할 수 있다. 따라서 개발자는 항상 보안 관련 사항을 염두에 두고 작업해야 한다.
	+ **코드 스타일 일관성**: 자동 생성된 코드가 일관성을 유지하지 않을 수 있다. 코드 스타일이 팀이나 프로젝트의 가이드라인과 일치하는지 확인하고 수정, 보완해야 한다.

---

## 컨텍스트 윈도우란?
컴퓨터가 한계나 이미지를 인식하는 기본 원리는 0과 1의 단계 이진 데이터를 처리하는 것에서 시작됩니다. 처음에는 텍스트와 동일한 형태의 데이터 형식을 인식하는 고정, 기술 개발에 따라 이미지와 비디오와 동일한 데이터를 인식하는 단계로 개선되었습니다. 이미지나 영상은 정적 연결되어 있는 정보를 포함하고 테스트하기 때문에 텍스트에 비해 훨씬 더 많은 양의 데이터를 포함하고 있습니다. 특히, 영상 데이터는 시간에 따라 달라지는 수많은 프레임을 처리해야 합니다.</br>
따라서 그 데이터의 양은 유일하게 이러한 양의 데이터를 처리하기 위해서는 기존의 방식대로 처리해야 하며, 보다 효율적인 데이터 처리 방법이 필요합니다. 이 과정에서 컨텍스트(Context Window)의 개념이 중요하게 작용합니다. 축소는 AI 모델이 한 번에 처리할 수 있는 데이터의 범위를 결정하며, 이 범위가 넓을수록 더 많은 정보를 동시에 처리하고 분석할 수 있습니다.   

컨텍스트 윈도우는 시퀀스 길이 또는 컨텍스트 길이라고도 하며, 언어 모델이 한 번에 처리하고 고려할 수 있는 최대 토큰 수를 나타냅니다.
이는 모델의 "메모리"를 나타내며, 일관된 응답을 이해하고 생성하는 데 얼마나 많은 이전 텍스트를 사용할 수 있는지 결정합니다.
더 큰 맥락 창을 통해 모델은 긴 구절에서도 일관성을 유지하고, 복잡한 내러티브를 이해하고, 광범위한 배경 정보가 필요한 작업을 처리할 수 있습니다.
컨텍스트 윈도우는 AI가 언어의 의미론과 구문을 이해하여 주어진 입력에 따라 관련성 있고 일관된 응답을 생성하는 데 필수적입니다. 
이는 모델이 처리할 수 있는 입력 시퀀스의 최대 길이를 결정하여 분석 및 이해를 위한 고정된 텍스트 범위를 제공합니다.
예를 들어, 챗봇 시나리오에서 맥락 길이가 길수록 AI는 대화의 더 많은 세부 사항을 기억할 수 있으며, 그 결과 상호작용이 더 맥락적으로 정확해집니다.

### Llama 3.1에서 토큰화가 작동하는 방식
토큰은 언어 모델이 처리하는 텍스트의 기본 단위입니다.
Llama 3.1의 맥락에서 토큰은 사용된 특정 토큰화 방법에 따라 단어, 단어의 일부 또는 단일 문자를 나타낼 수 있습니다.</br>

Llama 3.1은 텍스트를 더 작은 단위로 나누는 하위 단어 토큰화 방법을 사용합니다. 이 접근 방식을 사용하면 모델은 하위 단어 토큰을 결합하여 희귀하거나 어휘에서 벗어난 용어를 포함한 다양한 단어를 처리할 수 있습니다. 예를 들어, "토큰화"라는 단어는 "토큰"과 "화"와 같은 토큰으로 나눌 수 있습니다. 이 방법은 어휘 크기와 광범위한 단어를 표현하는 능력 간의 균형을 맞춥니다.

정확한 토큰화 프로세스는 복잡할 수 있지만 Llama 3.1에서 토큰 수를 추정하기 위한 몇 가지 일반적인 지침은 다음과 같습니다.

1. 평균적으로 토큰 하나는 영어 텍스트에서 약 4개의 문자에 해당합니다.
2. 영어의 일반적인 단어는 종종 1~2개의 토큰으로 표현됩니다.
3. 공백과 구두점은 일반적으로 별도의 토큰으로 계산됩니다.‑‑

### 컨텍스트 윈도우(Context Windows)의 중요성
컨텍스트 윈도우(Context Windows)가 AI 애플리케이션에 미치는 영향은 상당히 중요합니다. 특히 윈도우의 크기는 긴 텍스트를 이해하거나 광범위한 콘텐츠를 생성할 때 매우 중요합니다. 더 큰 윈도우를 사용하면 모델이 응답하기 전에 더 많은 정보를 고려할 수 있기 때문에 일관된 결과를 얻을 수 있습니다. 이는 주로 문서 요약이나 콘텐츠 생성 및 복잡한 질의응답 시스템과 관련이 있습니다.   

그러나 컨텍스트 윈도우(Context Windows)가 커질수록 더 많은 계산 능력과 메모리가 필요하기 때문에 성능과 리소스 효율성 사이의 균형을 유지하는 것이 중요합니다. 입력 토큰 수로 측정된 컨텍스트의 증가는 운영 비용에 직접적인 영향을 미칩니다. 또한 출력 토큰 수보다는 영향이 적지만 대기 시간에도 영향을 줍니다.

+ 최적화의 중요성 - 컨텍스트 윈도우의 최적 크기를 찾는 것은 AI 애플리케이션의 성능과 효율성에 직접적인 영향을 미치므로 매우 중요합니다. 크기를 계산 리소스와 작업 요구 사항과 균형 있게 맞추는 것이 AI 성능을 극대화하는 데 중요합니다.
+ 보안 및 윤리적 고려 사항 - AI 시스템이 더욱 발전함에 따라 그 기능의 보안 및 윤리적 영향, 특히 대규모 컨텍스트 창과 관련된 영향을 해결하는 것은 신뢰와 규정 준수를 유지하는 데 필수적입니다.
+ 미래 추세 - 이 분야는 다양한 컨텍스트 크기를 처리하거나 속도나 정확도를 떨어뜨리지 않고 효과적인 메모리를 확장하는 고급 기술을 활용할 수 있는 보다 적응력 있고 효율적인 모델을 향해 움직이고 있습니다.

컨텍스트 창은 언어 처리에서 AI의 기준 프레임 역할을 하기 때문에 AI 도구 의 기능과 효율성에 기초가 됩니다. 이는 AI 상호 작용의 일관성, 관련성 및 깊이에 
직접 영향을 미치며 AI 모델이 응답을 생성하거나 데이터를 분석하는 데 얼마나 많은 사전 입력을 고려할 수 있는지 결정합니다.

---

## 문서 요약 기능

### 1. Llama 3.1의 문서 요약 기능이란?
라마의 문서 요약기는 긴 문서를 더 짧고 소화하기 쉬운 버전으로 요약할 수 있는 기능입니다.</br>
주로 이 도구는 다음과 같은 용도로 적합합니다.
+ 긴 기사 또는 보고서를 빠르게 파악하기
+ 복잡한 문서에서 주요 시사점 찾기
+ 정보 과부하 차단
### 2. Llama 3.1의 문서 요약 기능 주요 기능
1. **자동 요약**: 광범위한 텍스트에서 핵심 사항과 주요 아이디어를 추출합니다.
2. **요약 생성**: 가장 중요한 정보를 강조하는 간단한 요약을 만듭니다.
3. **문장 수준 제어**: 관련성과 중요도에 따라 포함할 특정 문장을 선택할 수 있습니다.

### 3. Llama 3.1의 문서 요약 기능 작동방식
1. **텍스트 분석**: 라마 모델은 전체 입력 텍스트를 분석하여 식별합니다.
	+ 주요 개념 및 실체
	+ 중요한 이벤트 및 관계
	+ 주요 논거 또는 주장
2. **문장 순위**: 각 문장은 문서의 관련성, 중요성, 문서의 전반적인 의미에 대한 기여도를 기준으로 점수가 매겨집니다.
3. **요약문 생성**: 요약문을 구성하기 위해 가장 높은 점수를 받은 문장을 선택합니다.
	+ 간결한 버전(보통 원본 텍스트의 10~20%)
	+ 일관된 내러티브 또는 핵심 사항
4. **후처리**: 생성된 요약문은 다음과 같은 다양한 기술을 통해 개선됩니다.
	+ 문장 재작성 및 순서 변경
	+ 중복 정보 제거

### 4. Llama 3.1의 문서 요약 기능 작동원리
라마의 문서 요약 기능은 자연어 처리(NLP)를 사용하여 텍스트를 간소화합니다. 작동 원리를 요약하면 다음과 같습니다

1. **토큰화**: 입력 텍스트는 토큰이라고 하는 개별 단어로 나뉩니다.
2. **POS(Part-of-Speech) 태깅**: 각 토큰은 문법적 맥락을 이해하기 위해 명사, 동사, 형용사, 부사 등으로 식별됩니다. </br>
(POS 태깅이란 문장 내 단어들의 품사를 식별하여 태그를 붙여주는 것을 말합니다.)
3. **개체명 인식(Named Entity Recognition(NER))**:  모델은 텍스트에서 사람, 조직, 위치 및 날짜와 같은 특정 객체를 식별합니다.</br>
(개체명 인식이란 텍스트문장에서 개체를 찾아서 미리 정의된 범주(사람 이름, 조직, 위치, 시간 , 날짜 등)으로 분류하는 자연어 처리(NLP) 방법입니다.)</br>
4. **의존 구문분석(Dependency Parsing)**: 주어-동사 관계와 같이 토큰이 어떻게 연관되는지 이해하기 위해 문장의 구조를 분석합니다.</br>
(의존 구문분석이란 문장보다는 문장에 존재하는 개별 단어 간 의존 또는 수식 방향으로 관계를 파악하여 문장 구조를 분석하는 방법입니다.)
5. **Semantic Role Labeling(SRL)**: 문장에서 개체의 역할을 식별하여 "누가 무엇을 했나요?"와 같은 질문에 답합니다.</br>
(SRL이란 자동적으로 문장 내에 논항(Argument)과 술어(또는 동사(Verb))(Predicate)를 분석하여 문장 내 성분에 시멘틱 롤 (Semantic role)(또는 의미 역할, 의미역)을 부여하는 방법입니다.)
6. **텍스트랭크 알고리즘(TextRank)**: 그래프 기반 알고리즘을 사용하여 텍스트 내의 연결을 기반으로 각 토큰과 문장의 중요도를 평가합니다. </br>
(TextRank는 PageRank(하이퍼링크를 가지는 웹 페이지에 대해서 얼마나 참조가 됐는지, 또는 유입이 되었는지 등으로 페이지의 순위를 매기는 알고리즘)의 알고리즘을 활용한 것으로, 페이지의 개념을 단어의 개념으로 바꾼 알고리즘입니다. 즉, 텍스트로 이루어진 글에서 특정 단어가 다른 문장과 얼마만큼의 관계를 맺고 있는지를 계산하는 것입니다.)
7. **그래프 기반 요약(Graph-Based Summarization)**: 요약을 나타내는 연결된 하위 그래프를 형성하여 가장 중요한 문장과 토큰을 선택합니다.

라마는 이 구조화된 정보를 사용하여 원본 문서의 주요 아이디어와 지원 세부 사항을 정확히 파악하여 명확하고 간결한 요약을 제공합니다.

### 5. Llama 3.1의 문서 요약 기능 테스트

1. 영어 뉴스
![질문](https://github.com/user-attachments/assets/c0cad542-742e-43d3-b5c2-55e738f9dde3)
출처 : By Deidre McPhillips/"Free Covid-19 tests are available again. Here’s how to get them"/CNN/2024,09.26
![답변](https://github.com/user-attachments/assets/338f6d64-bee0-4752-a2f6-37714db7e585)

2. 영어 논문
![image](https://github.com/user-attachments/assets/dfab6cf8-76b7-45e8-8e49-b978eebc51c9)
출처 : Li Yang, Shasha Liu, Jinyan Liu, Zhixin Zhang, Xiaochun Wan, Bo Huang, Youhai Chen & Yi Zhang/"COVID-19: immunopathogenesis and Immunotherapeutics"/Signal Transduction and Targeted Therapy/ 5, Article number: 128 (2020)/p.2
![image](https://github.com/user-attachments/assets/841e07ef-2d39-47c3-b940-286aae0e0774)

3. 한국어 뉴스
![image](https://github.com/user-attachments/assets/40b472e5-bff9-4882-ae05-01fb940870f7)
출처 : 내 손안에 서울/"황홀한 가을밤…서울세계불꽃축제! 교통통제 구간은?"/내 손안에 서울/2024.10.02
![image](https://github.com/user-attachments/assets/fd358620-f029-4c17-886d-5e8e988dda9f)

4. 한국어 논문
![image](https://github.com/user-attachments/assets/8c386679-c24c-47db-b003-42c036201f48)
출처 : 황응수,박정규, 차창용/"바이러스 감염에 대한 면역반응"/대한면역학회/2004년/p.74
![image](https://github.com/user-attachments/assets/40579734-a9db-4e49-91ef-b7c92ff62637)

전반적으로는 우수한 성능을 보였으나 특정 분야에 대한 전문 용어와 지식이 포함된 문서에서는 다소 부족한 성능을 보였습니다.
한국어 기능도 지원은 하나 영어로 된 문서와 비교했을 때는 다소 부족한 성능을 보였습니다.

### 6. 제한 사항
1. **텍스트 복잡성**: 라마의 문서 요약기는 명확하게 구조화된 문서에서 가장 잘 작동합니다. 그러나 모호하거나 제대로 정리되지 않은 내용으로 인해 어려움을 겪을 수 있습니다.
2. **도메인 지식**: 다른 NLP 모델과 마찬가지로 특정 분야에 대한 전문 용어와 지식에 따라 성능이 달라질 수 있습니다.
3. **맥락적 고려**: 일부 정보는 문맥이나 뉘앙스에 따라 다르며, 모델이 이러한 세부 정보를 완전히 포착하지 못할 수도 있습니다.

---

<a name="similar_LLM"></a>
# 5. 유사한 기능의 LLM
>## Chat GPT
자연어 처리의 새로운 지평을 보여주었고, 인공지능의 엄청난 발전을 보여주었으며, 현재도 가장 상용화되어 자연어처리 인공지능의 대명사로 불리우는 AI이다.
자연어 처리 뿐만 아니라 이미지 처리, 음성 처리 역시 뛰어난 역량을 보여주지만, 완벽한 기능을 사용하기 위해서는 유료 결제가 필수적이며, 사용자의 사용 편의 및 전문성을 올려주는 파인튜닝을 하기 위해서 추가적인 비용이 발생한다. 이는 오픈소스 소프트웨어와 비교하면 부족한 부분이라고 할 수 있지만, 일반 사용자의 편의성이 높은 점과 쉬운 접근성은 아주 큰 강점이라고 할 수 있다.

>## Qwen
알리바바에서 공개한 자연어 처리 모델이다.</br>
모기업인 알리바바가 중국 기업인만큼, 중국어 처리에 있어서는 다른 어떤 모델보다 강력한 성능을 보여주며, 다른 자연어 처리 인공지능에서 제공하는 대부분의 기능을 높은 수준에서 보여준다.
또한, 오픈소스 소프트웨어로 일부 모델을 제외한 모든 모델이 Apache 2.0 라이센스를 따르기 때문에, 사용자의 필요에 따라 일부 부분을 수정하여 파인튜닝을 비롯한 여러 기능을 활용할 수 있다.
LLaMA에 비해 더 적은 차원으로 추론을 하는 만큼, 필요한 상황에 대한 파인튜닝이 더욱 빠르고 강력하게 작용하여 더 적은 데이터와 더 적은 컴퓨팅 리소스로도 비슷한 성능을 기대할 수 있다.
하지만, LLaMA에 비해 적은 차원은 복잡한 언어 표현에 약한 모습을 보이며, 상대적으로 적은 레이어는 잘못된 데이터에 의한 과적합이 발생할 가능성을 높일 수 있는 위험성을 내포하고 있다.

>## Claude
클로드(Claude)는 앤트로픽(Anthropic)에서 개발한 대형 언어 모델 제품군이다.</br>
claude 3 Opus까지의 기준으로는 작문과 연관된 능력, 다국어에 대한 이해도가 출시 당시 기준 다른 언어 모델보다 뛰어난 편이다. 특히 유료 버전의 기계 번역의 성능이 속도를 제외하면 정확성 측면에서 매우 뛰어나다.
파인 튜닝 없이도 질문의 핵심을 캐치하는 능력이 좋다.답변도 캐치해낸 핵심을 기준으로 되도록 간결하고 명확하게 설명하는 편이다.
일정한 시간안에 작성한 채팅수가 초과되면 일정시간 채팅이 차단된다. 새로운 채팅창을 열어도 풀리지 않는다. 유료 버전은 양을 조금 더 주긴 하지만 3.5 모델은 여전히 사용량이 작은 느낌이다.

>## Gemini
Gemini는 구글과 딥마인드가 개발한 멀티모달(LMM) 생성형 인공지능 모델이다. 텍스트뿐만 아니라 오디오, 이미지, 비디오와 같은 다양한 입출력을 지원한다.</br>
멀티모달 입력 처리 llama3.1보다 텍스트, 이미지, 음성 등 여러 입력 모드를 처리하는 데 탁월하여 풍부하고 다양한 사용자 상호작용이 필요한 애플리케이션에 매우 다양하게 활용할 수 있습니다.
구체적인 성능 비교에서는 llama 3.1이 특정 상황에서 효율성과 정밀성 면에서 우위를 점할 수 있음을 보여줍니다. 특히, 8B와 70B 매개변수 버전이 일부 벤치마크 테스트에서 Gemini 1.5 Pro보다 성능이 우수했습니다.
더 제한적인 라이선스로 인해 개발자가 Llama 3.1처럼 자유롭게 모델을 수정하거나 실험할 수 있는 유연성이 제한될 수 있습니다.

>## copilot
Microsoft Copilot은 Microsoft Office 제품군에서 사용되는 AI 도우미로, 사용자 생산성 향상을 위한 도구입니다. 이 도구는 사용자의 명령을 이해하고 문서 작성, 분석, 이메일 작성 등의 작업을 지원합니다.<\br>
Microsoft Copilot은 사용자의 생산성을 극대화하는 도구로, Office 제품군과의 긴밀한 통합을 통해 문서 작성, 데이터 분석, 일정 관리 등을 자동화하여 효율성을 높입니다. 클라우드 기반으로 데이터를 안전하게 관리하고 어디서나 접근할 수 있다.
Microsoft Copilot은 사용자가 특정 작업에 대한 질문을 할 때, 그에 대한 답변과 함께 근거 자료 또는 참고 코드나 링크를 제공해 사용자가 그 내용을 확인하고 추가로 검증할 수 있도록 돕습니다.
상용 서비스로 구독 요금이 발생해 개인 사용자나 소규모 기업에게는 부담이 될 수 있습니다. 또한, Copilot은 Microsoft 제품군에 최적화되어 있어, Office 환경을 사용하지 않거나 다른 소프트웨어를 사용하는 경우 기능을 충분히 활용하기 어렵습니다.

---

<a name="future"></a>
# 6. 인공지능의 방향성
자연어 처리 모델이 확장해 나갈 새로운 방향은 몇가지가 있다.
### SLM(Small Language Model)
우선은 더 작은 데이터 규모에서도 좋은 성능을 보여주는 SLM(Small Language Model)이다.</br>
현재 일반적으로 사용하는 자연어처리 모델은 LLM(Large Language Model)로 대규모의 자연어 데이터와 더 많은 레이어, 차원을 이용하여 언어처리를 하는 방식이다.
LLaMA(Large Language Model Meta AI) 역시 이름에서도 LLM임을 드러나고, 다른 대규모 언어 모델 역시 LLM 방식을 사용하는 것에서 알 수 있듯, 성능 면에서는 다른 방식의 추종을 불허하고 있다.   

하지만, LLM을 이용한 학습의 기반이 되는 데이터는 한정 되어있고, 이미 사용하는 데이터의 양이 생성되는 데이터의 양을 넘어서고 있다.
AI를 통해 생성된 데이터를 이용하는 방법도 있을 수 있으나, 이는 AI에게 과적합을 유도하는 방법이며, AI는 사람이 아니기에 AI에 의해 생성된 데이터로 학습하는 것은 점차 사람답지 않게되는 결과를 불러오게 된다.</br>
이 뿐만 아니라, 대규모 학습 및 거대 구조를 시뮬레이션하기 위해 소모되는 컴퓨팅 리소스 역시 큰 문제이다. 이미 수많은 데이터 센터에서 사용되는 에너지는 소규모 국가의 전력 소모량을 넘어섰고, 2027년 경에는 전체 데이터 센터의 전력 소모량이 아르헨티나, 스웨덴, 네덜란드의 소비량이 될 것으로 예상되고 있다.
한 연구에 따르면 챗GPT의 핵심 기술인 ‘거대언어모델’을 학습하는데 들어가는 전기량은 미국 120개 가구의 1년 전기 사용량 정도이다.
또한, ‘완두콩이 들어가지 않은 볶음밥을 그려줘’나, ‘구운 연어를 넣은 볶음밥을 만들어서 태워줘’라는 식의 이미지를 생성할 때, 이미지 하나당 핸드폰이 완충되는 정도의 전기가 소모된다.
이는 현재 글로벌 친환경 정책에 명백히 위배되며, 시장이 아주 빠르게 성장하는 몇 년이 지나면 반드시 마주하게 될 거대한 문제이다.   

이러한 문제를 해결하는 방법으로 제시되는 것이 SLM이다.</br>
더 작은 구조를 이용하기에 모든 논리 상황이나 언어 개념을 이해할 수는 없지만, 특정한 분야에 더욱 집중하여 학습에 필요한 데이터를 줄이고, 연산에 필요한 컴퓨팅 자원의 소모를 획기적으로 줄일 수 있다.   
또한, 이는 낙관적인 예측이지만, SLM과 오픈소스의 만남으로 더 많은 발전을 기대할 수도 있다.</br>
현재의 LLM은 대규모의 GPU를 사용하더라도 학습에 며칠에서 몇개월의 시간이 필요하지만, SLM은 그보다 작은 규모에서도 도전할 수 있다.
이런 이점이 오픈소스와 만나게 된다면, 모든 사람이 직접 학습 시키고, 개선을 시도할 수 있으며, 더욱 자유롭고 민주적인 발전을 기대할 수 있다.

### 멀티 모달 모델
우리가 일반적으로 기대하는 대화형 인공지능의 결정체이다.</br>
텍스트 인식은 기본으로 가능하고, 음성 인식, 이미지 인식, 동영상 인식을 넘어 수많은 데이터 환경을 이해 및 분석 가능하며 데이터 간의 연계 및 상호 활용이 가능한 모델을 의미한다.
기존의 언어모델이 텍스트 데이터 처리와 생성에 특화되어 있으며, 다른 데이터 역시 텍스트로 변환하여 처리한다.
반면 멀티모달 모델은 여러 데이터를 동시에 학습하는 것으로 여러가지 데이터 입출력을 받아들일 수 있으며, 정보를 종합적으로 이해하고 통합하는 것이 가능하다.    
비유하자면, 언어모델은 세상을 글자의 집합으로 생각하며, 각각의 글자 간의 관계만을 파악한다면, 멀티모달 모델은 각각의 객체를 이미지와 동시에 글자로 파악하며 우리가 상상하는 것에 좀 더 가까운 방식이라고 할 수 있다.

이러한 멀티모달 모델이 필요한 이유는, 현재 AI와 로봇의 융합을 각 분야의 최종점으로 보고 있으며, 실제 open AI와 협업을 하는 Figure(피규어)에서 이미 시도하고 있다.
AI의 데이터 수집 및 처리 능력을 로봇의 수많은 센서와 만나 실시간 데이터 처리 기능이 만개할 수 있다.
로봇 역시 자체적인 제어 시스템을 넘어서, AI를 이용한 실시간 적응형 제어 시스템을 도입하여 제어 기능의 극화를 이룰 수 있다.
