01 협력하는 객체들의 공동체
==========================

*시너지를 생각하라. 전체는 부분의 합보다 크다.*<br>
-스티븐 코비(Stephen R. Covey)

&nbsp;객체지향에서 실세계의 모방이라는 개념은 유연하고 실용적인 관점에서 객체지향을 설명하기에는 적합하지 않다. 객체지향의 목표는 실세계의 모방이 아닌 새로운 세계의 창조다.

하지만 객체를 현실 세계의 생명체에 비유하는 것은 상태와 행위를 '캡슐화(encapsulation)'하는 소프트웨어 객체의 '자율성(autonomous)'을 설명하는 데 효과적이기 때문이다. 현실 세계의 사람들은 '메시지(message)'를 주고받으며 공동의 목표를 달성하기 위해 '협력(collaboration)'하는 객체들의 관계를 설명하는 데 적합하다. 이는 객체지향 설계의 핵심 사상인 '연결완전성(seamlessness)'을 설명하는 데 적합하다.

## 협력하는 사람들

### 커피 공화국의 아침
&nbsp;샐러리맨들이 출근해서 커피를 마시는 상황은 손님, 캐시어, 바리스타 사이의 암묵적인 협력 관계가 존재한다. 또한 다들 각각의 역할이 존재하며 자신이 맡은 바 책임을 다한다. 협력에 참여하는 모든 사람들은 역할과 책임을 다하고 있다.

### 요청과 응답으로 구성된 협력
&nbsp;사람들은 스스로 해결하지 못하는 문제와 마주치면 다른 사람에게 도움을 요청(request)한다. 또한 요청은 연쇄적으로 발생한다.

* 손님이 캐시어에게 커피를 제공해 줄 것을 요청한다.
* 캐시어가 바리스타에게 주문된 커피를 제조해줄 것을 요청한다.

요청을 받은 사람은 다른 사람 요청에 응답(response)한다. 응답 역시 요청의 방향과 반대 방향으로 연쇄적으로 전달된다.

* 바리스타는 캐시어에게 커피제조가 완료됐음을 알려주는 응답을 한다.
* 캐시어는 손님에게 진동벨을 통해 손님의 주문에 응답한다.

요청과 응답을 통해 다른 사람과 협력(collaboration)할 수 있는 능력은 거대하고 복잡한 문제를 해결할 수 있는 공동체를 형성할 수 있게 만든다.

### 역할과 책임
&nbsp;사람들은 역할(role)을 부여받는다. 커피공화국에서는 '손님', '캐시어', '바리스타'라는 역할이 존재한다. 역할이라는 단어는 의미적으로 책임(responsibility)이라는 개념을 내포한다. 따라서 특정한 역할은 특정한 책임을 암시한다.

손님은 커피를 주문할 책임, 캐시어는 주문 내용을 바리스타에게 전달하고 커피가 준비됐다는 사실을 손님에게 알릴 책임, 바리스타는 커피를 제조할 책임이 있다. 역할과 책임은 협력의 핵심적인 구성 요소다. 이로인해 몇가지 중요한 개념이 나온다.

* 여러 사람이 동일한 역할을 수행할 수 있다.
* 역할은 대체 가능성(substitutable)을 의미한다.
* 책임을 수행하는 방법은 자율적으로 선택할 수 있다.
* 한 사람이 동시에 여러 역할을 수행할 수 있다.

## 역할, 책임, 협력

### 기능을 구현하기 위해 협력하는 객체들
&nbsp;커피 주문 과정에서 사람을 객체로, 요청을 메시지로, 요청을 처리하는 방법을 메서드로 바꾸면 객체지향이라는 문맥으로 옮길 수 있다. 이것이 객체지향을 설명할 때 실세계 모방이라는 표현을 사용하는 이유다.

### 역할과 책임을 수행하며 협력하는 객체들
*어떤 객체도 섬이 아니다*<br>
&nbsp;워드 커니험(Ward Cunningham)과 켄트 벡(Kent Beck)의 말을 인용한 것이다. 객체는 주어진 역할과 책임을 다하는 동시에 시스템의 더 큰 목적을 이루기 위해 다른 객체와도 적극적으로 협력한다. 그리고 사용자가 최종적으로 인식하게 되는 시스템의 기능은 객체들이 협력해서 일궈낸 결실이다.

결론적으로 시스템은 역할과 책임을 수행하는 객체로 분할되고 객체 간의 연쇄적인 요청과 응답으로 구성된 협력에 의해 구현된다.

객체지향 설계는 적절한 객체에게 적절한 책임을 할당하는 것에서 시작한다. 역할은 관련성 높은 책임의 집합이며, 다음과 같은 특성을 지닌다.

* 여러 객체가 동일한 역할을 수행할 수 있다.
* 역할은 대체 가능성을 의미한다.
* 각 객체는 책임을 수행하는 방법을 자율적으로 선택할 수 있다.
* 하나의 객체가 동시에 여러 역할을 수행할 수 있다.

## 협력 속에 사는 객체
&nbsp;객체지향 애플리케이션에서 실제로 협력에 참여하는 주체는 객체다. 객체가 없는 객체지향 세계는 아무런 의미가 없다. 객체지향이라고 부르는 이유는 패러다임의 중심에는 객체가 있기 때문이다.

객체는 다음과 같은 두 가지 덕목을 갖춰야 한며, 두 덕목 사이에서 균형을 유지해야 한다.

첫째, 객체는 충분히 '협력적'이어야 한다. 외부의 도움없이 작동하는 전지전능한 객체는 내부적인 복잡도에 의해 자멸하고 만다. 충분히 협력적은 수동적인 존재가 아니다. 명령에 복종하는 것이 아닌 요청에 응답할 뿐이다. 심지어 요청에 응할지 여부도 스스로 결정할 수 있다.

둘째, 객체는 충분히 '자율적'이어야 한다. 어떤 사물이 자신의 행동을 스스로 결정하고 책임진다면 우리는 그 사물을 자율적인 존재라고 말한다. 사람들은 다른 사람의 요청에 따라 행동하지만 최대한 스스로의 판단에 따라 결정하고 행동한다. 객체도 유사하다.

### 상태와 행동을 함께 지닌 자율적인 객체
&nbsp;흔히 객체를 상태(state)와 행동(behavior)을 함께 지닌 실체라고 정의한다. 이는 객체가 협력에 참여하기 위해 어떤 행동을 해야한다면 상태도 함께 지니고 있어야 한다는 것을 의미한다. 객체가 자율적인 존재로 남기 위해서는 행동과 상태를 함께 지니고 있어야 한다.

객체의 자율성은 객체의 내부와 외부를 명확하게 구분하는 것으로부터 나온다. 객체는 다른 객체가 '무엇(what)'을 수행하는지는 알 수 있지만 '어떻게(how)'수행하는지에 대해서는 알 수 없다. 따라서 객체는 상태와 행위를 하나의 단위로 묶는 자율적인 존재다.

### 협력과 메시지
&nbsp;객체지향의 세계에서는 메시지라는 의사소통 수단만이 존재한다. 따라서 협력은 메시지를 전송하는 객체와 수신하는 객체 사이의 관계로 구성된다. 메시지를 전송하는 객체는 송신자(sender), 메시지를 수신하는 객체는 수신자(receiver)라고 부른다.

### 메서드와 자율성
&nbsp;송신자가 메시지를 전송하고 수신자가 메시지를 처리하는 방법을 메서드(method)라고 부른다. 객체지향 프로그래밍 언어에서 메서드는 클래스 안에 포함된 함수 또는 프로시저를 통해 구현된다. 메시지와 메서드의 분리는 객체들 간의 자율성을 증진시킨다.

메시지와 메서드를 분리하는 것은 자율성을 높이는 핵심 메커니즘이며, 캡슐화(encapsulation)라는 개념과 깊이 관련돼 있다.

## 객체지향의 본질
&nbsp;다음은 객체 지향의 개념을 간략하게 정리한 것이다.

* 객체지향이란 시스템을 상호작용하는 자율적인 객체들의 공동체로 바라보고 객체를 이용해 시스템을 분할하는 방법이다.
* 자율적인 객체란 상태와 행위를 함께 지니며 스스로 자기 자신을 책임지는 객체를 의미한다.
* 객체는 시스템의 행위를 구현하기 위해 다른 객체와 협력한다. 각 객체는 협력 내에서 정해진 역할을 수행하며 역할은 관련된 책임의 집합이다.
* 객체는 다른 객체와 협력하기 위해 메시지를 전송하고, 메시지를 수신한 객체는 메시지를 처리하는 데 적합한 메서드를 자율적으로 선택한다.

### 객체를 지향하라
&nbsp;많은 사람들은 객체지향을 클래스를 지향하는 것으로 생각했다. 클래스가 객체지향 프로그래밍 언어 관점에서 매우 중요한 구성요소인것은 분명하지만 객체지향의 중심 개념이라고 말하기에는 무리가 있다. JS와 같은 프로토타입(prototype)기반의 언어에서는 오직 객체만이 존재한다. 상속 역시 객체 간의 위임(delegation) 메커니즘을 기반으로 한다. 클래스는 객체를 만드는 데 필요한 구현 메커니즘일 뿐이다.

객체의 역할, 책임, 협력에 집중하라. 객체지향은 객체를 지향하는 것이지 클래스를 지향하는 것이 아니다.