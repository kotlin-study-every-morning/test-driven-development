<aside>
💡 질문에 대한 답이나 힌트 얻기

</aside>

### 단계 얼마나 커야하나?

---

- 각 테스트가 다뤄야 할 범위는 얼마나 넓은가?
    - 테스트의 크기는 천차만별이다.
- 리팩토링을 하면서 얼마나 중간 단계를 거쳐야하는가?
    - 초기에는 아주 작은 단계로 작업
    - 많은 작은 리팩토링 작업 후 몇단계 건너뛰는 시도
    - 자동화된 리팩토링 ide 툴 활용

### 테스트 할 필요가 없는 것은?

---

두려움이 지루함으로 변할때까지 테스트 만들기

- 내가 작성한 코드에 대해서만 작성
- 불신한 이유가 없다면 다른 사람이 만든 코드 테스트하지 마라. ⇒ 코드가 바뀐다면 테스트가 실패하기 때문

### **좋은 테스트를 갖췄는지의 여부를 어떻게 알 수 있는가?**

---

설계문제가 있을을 알려주는 테스트 속성

- 긴 셋업코드 - 객체가 너무 크므로 나눌 필요가 있다.
- 셋업 중복 - 서로 밀접하게 엉킨 객체들이 많음
- 실행시간이 오래 걸리는 테스트 - 작은 부분만 따로 테스트 하기 힘들다는 뜻으로 설계 문제가 있을 수 있다.
- 깨지기 쉬운 테스트 - 다른 부분이 영향을 끼친다는 것이다.

### **피드백이 얼마나 필요한가? ??**

---

테스트를 얼마나 작성할지 고려할때 실패간 평균시간(Mean Time Between Failures 실패하는 시점간의 평균시간차)을 고려

<aside>
💡 mtbf
어떤 시스템이 결함으로 인해 실패할 때 실패하는 시점 간의 평균 시간차
예시 ⇒ 어떤 웹사이트가 평균적으로 하루에 한 번 오작동 한다면 그사이트의 mtbf는 24시간

</aside>

- 정말 일어날 것 같지 않은 조건 테스트가 의미가 있냐 없냐?
    - 정수형 테스트하는데 MAXINT에 관해서 테스트 하는것은 의미가 없음
    - 하지만 심장 박동 조절 장치에 대한 테스트를 10년에서 100년으로 바꾸는건 의미가 있을 수 있음
- 결국은 TDD의 테스트에 대한 관점은 실용적이고 어떤 목적을 위한 하나의 수단이다.

### 거대한 시스템을 개발할때도 TDD를 할 수 있는가?

---

- 시스템에 있는 기능의 양은 TDD의 효율에 영향을 미치지 않음.
- 중복을 제거함에 더 작은 객체들이 만들어지고
- 애플리케이션 크기와 무관하게 독립적으로 테스트 될수 있음.

### **애플리케이션 수준의 테스트로도 개발을 주도할 수 있는가?**

---

- 작은 규모의 테스트로 개발을 주도하는것의 문제는 실제로 사용자가 원하지 않는데 그들이 원할 거라 생각하고 구현할 수도 있는 위험을 끌고 간다는 점에 있다. ⇒ 실제 예시가 아닌 개발자가 만들어낸 예제를 통해서 한다.?? 아니면 메서드만 테스트하기 때문에 예상치 못한 위험이 있다??
- 어플리케이션 수준에서 테스트 작성한다면 사용자가 원하는 바를 테스트로 작성할 수 있다. ⇒ e2e?

일반적인 ATDD(Acceptance Test Driven Development - 인수 테스트)

개발자, 테스터 간의 커뮤니케이션을 기반으로 하는 개발 방법론으로써, 고객-개발자-테스터 간의 협업을 강조

책에 있는 ATDD(application test driven development)

테스트를 작성하는 것은 사용자에게는 기존에 없던 새로운 책임이 되는 것이다. ⇒ 어플리케이션 테스트를 우선적으로 작성하기 위해서는 협조 노력이 필요하다.

### **프로젝트 중반에 TDD를 도입하려면 어떻게 해야 할까?**

---

테스트를 염두에 두지 않고 만든 코드는 테스트하기가 쉽지 않다.

⇒ 이 얘기 자체가 어플리케이션 만드는데 말고 프레임워크 자체를 만든다는 가정 같음

확실히 하지 말아야할 일

- 코드 전체를 위한 테스트를 한꺼번에 다 만들고 한번에 리팩토링하는일 ⇒ 리팩토링하기가 힘들듯

해야할 것들

- 변경의 범위 제한
- 테스트와 리팩토링 사이에 존재하는 교착상태 deadlock을 풀어주는것
- 피드백을 얻어 작성

### **TDD는 누구를 위한 것인가?**

---

더 깔끔한 설계를 할 수 있도록, 설계를 개선하며 적절한 문제에 집중할 수 있게 도와줌

프로젝트가 80%만 어느정도 적당한 수준으로 엔지니어되어도 성공이하고 할 수있지만,

TDD는 통용되는 수준보다 월씬더 깨끗한 설계의 코드를 더 적은수의 결함으로 구현되게 만듦

'엄청난 흥미를 가지고 새 프로젝트를 시작해서 시간이 지남에 따라 서서히 코드가 썩어가는걸 보게된다.'

테스트가 쌓여감에 따라 코드에 대한 자신감이 생기며 설계를 개선해나가면서 점점 더 많은 설계변경을 가능하게 함.

### **어째서 TDD가 잘 작동하는가?**

---

이 결과의 일부는 분명 결함 감소에서 온다. 결함을 빨리 발견해 고칠수록 비용은 낮아짐.

### **TDD와 익스트림 프로그래밍의 실천법 사이에 어떤 관련이 있는가?**

---

- 짝 프로그래밍 - 짝으로 일하는 것은 TDD를 강화시킴. 그 리듬 때문에 몰입할 수있으며, 지칠때 활기를 줌. 작성하게 되는 테스트는 짝 프로그래밍 과정에서 좋은 의사소통 수단이 됨
- 활기차게 일하기
- 지속적인 통합
- 단순 설계
- 리팩토링
- 지속적인 전달 - 위험부담 줄임