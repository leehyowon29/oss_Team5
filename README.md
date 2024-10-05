# 제목 : LLaMA 3 에 대해 알아보자
---
## 팀원 :
+ 통계학과/2024009762/이효원 (leehyowon29)
+ 모바일공학과/2024007587/홍은지 (zjseltus)
+ 통계학과/2024002789/이한선 (ihanseon)
+ 전자공학부 인공지능전공/2024003120/이가령 (GaRyeong9828)
---
# 1. Introduction
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
# 2. License

---
# 3. ...
컨텍스트 윈도우란?
컨텍스트 윈도우는 시퀀스 길이 또는 컨텍스트 길이라고도 하며, 언어 모델이 한 번에 처리하고 고려할 수 있는 최대 토큰 수를 나타냅니다.
이는 모델의 "메모리"를 나타내며, 일관된 응답을 이해하고 생성하는 데 얼마나 많은 이전 텍스트를 사용할 수 있는지 결정합니다.
더 큰 맥락 창을 통해 모델은 긴 구절에서도 일관성을 유지하고, 복잡한 내러티브를 이해하고, 광범위한 배경 정보가 필요한 작업을 처리할 수 있습니다.
컨텍스트 윈도우는 AI가 언어의 의미론과 구문을 이해하여 주어진 입력에 따라 관련성 있고 일관된 응답을 생성하는 데 필수적입니다. 
이는 모델이 처리할 수 있는 입력 시퀀스의 최대 길이를 결정하여 분석 및 이해를 위한 고정된 텍스트 범위를 제공합니다.
예를 들어, 챗봇 시나리오에서 맥락 길이가 길수록 AI는 대화의 더 많은 세부 사항을 기억할 수 있으며, 그 결과 상호작용이 더 맥락적으로 정확해집니다.

Llama 3.1에서 토큰화가 작동하는 방식
토큰은 언어 모델이 처리하는 텍스트의 기본 단위입니다.
 Llama 3.1의 맥락에서 토큰은 사용된 특정 토큰화 방법에 따라 단어, 단어의 일부 또는 단일 문자를 나타낼 수 있습니다.

Llama 3.1은 텍스트를 더 작은 단위로 나누는 하위 단어 토큰화 방법을 사용합니다. 이 접근 방식을 사용하면 모델은 하위 단어 토큰을 결합하여 희귀하거나 어휘에서 벗어난 용어를 포함한 다양한 단어를 처리할 수 있습니다. 예를 들어, "토큰화"라는 단어는 "토큰"과 "화"와 같은 토큰으로 나눌 수 있습니다. 이 방법은 어휘 크기와 광범위한 단어를 표현하는 능력 간의 균형을 맞춥니다.

정확한 토큰화 프로세스는 복잡할 수 있지만 Llama 3.1에서 토큰 수를 추정하기 위한 몇 가지 일반적인 지침은 다음과 같습니다.

1,평균적으로 토큰 하나는 영어 텍스트에서 약 4개의 문자에 해당합니다.
2,영어의 일반적인 단어는 종종 1~2개의 토큰으로 표현됩니다.
3,공백과 구두점은 일반적으로 별도의 토큰으로 계산됩니다.‑‑


컨텍스트 윈도우(Context Windows)의 중요성
컨텍스트 윈도우(Context Windows)가 AI 애플리케이션에 미치는 영향은 상당히 중요합니다. 특히 윈도우의 크기는 긴 텍스트를 이해하거나 광범위한 콘텐츠를 생성할 때 매우 중요합니다. 더 큰 윈도우를 사용하면 모델이 응답하기 전에 더 많은 정보를 고려할 수 있기 때문에 일관된 결과를 얻을 수 있습니다. 이는 주로 문서 요약이나 콘텐츠 생성 및 복잡한 질의응답 시스템과 관련이 있습니다.

그러나 컨텍스트 윈도우(Context Windows)가 커질수록 더 많은 계산 능력과 메모리가 필요하기 때문에 성능과 리소스 효율성 사이의 균형을 유지하는 것이 중요합니다. 입력 토큰 수로 측정된 컨텍스트의 증가는 운영 비용에 직접적인 영향을 미칩니다. 또한 출력 토큰 수보다는 영향이 적지만 대기 시간에도 영향을 줍니다.

주요 

⦁최적화의 중요성 - 컨텍스트 윈도우의 최적 크기를 찾는 것은 AI 애플리케이션의 성능과 효율성에 직접적인 영향을 미치므로 매우 중요합니다. 크기를 계산 리소스와 작업 요구 사항과 균형 있게 맞추는 것이 AI 성능을 극대화하는 데 중요합니다.
⦁보안 및 윤리적 고려 사항 - AI 시스템이 더욱 발전함에 따라 그 기능의 보안 및 윤리적 영향, 특히 대규모 컨텍스트 창과 관련된 영향을 해결하는 것은 신뢰와 규정 준수를 유지하는 데 필수적입니다.
⦁미래 추세 - 이 분야는 다양한 컨텍스트 크기를 처리하거나 속도나 정확도를 떨어뜨리지 않고 효과적인 메모리를 확장하는 고급 기술을 활용할 수 있는 보다 적응력 있고 효율적인 모델을 향해 움직이고 있습니다.

컨텍스트 창은 언어 처리에서 AI의 기준 프레임 역할을 하기 때문에 AI 도구 의 기능과 효율성에 기초가 됩니다. 이는 AI 상호 작용의 일관성, 관련성 및 깊이에 직접 영향을 미치며 AI 모델이 응답을 생성하거나 데이터를 분석하는 데 얼마나 많은 사전 입력을 고려할 수 있는지 결정합니다.
