다음과 같은 보고서가 있습니다

| 종목  | 주    | 가격  | 합계    |
|-----|------|-----|-------|
| IBM | 1000 | 25  | 25000 |
| GE  | 400  | 100 | 40000 |
|     |      | 합계  | 65000 |

다중 통화를 지원하는 보고서를 만들려면 통화 단위를 추가해야합니다

| 종목       | 주    | 가격     | 합계       |
|----------|------|--------|----------|
| IBM      | 1000 | 25USD  | 25000USD |
| Novartis | 400  | 100CHF | 60000CHF |
|          |      | 합계     | 65000    |

또한 통화 단위의 환률도 표시해야겠죠?

| 기준  | 변환  | 환률  |
|-----|-----|-----|
| CHF | USD | 1.5 |

새로운 보고서를 작성하려고 하면 어떤 기능들이 있어야할까요? 즉, 어떤 테스트가 필요해야할까요?

- 통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을 수 있어야 한다
- 어떤 금액(주가)을 어떤 수(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야한다

우리가 테스트해야할 목록들입니다

- %5 + 10CHF = $10(환율이 2:1일 경우)
- $5 * 2 = $10
- amount를 private 으로 만들기
- Dollar 부작용?
- Money 반올림?

테스트를 작성할 때는 오퍼레이션(메서드 / 객체가 수행할 수 있는 연산)의 완벽한 인터페이스에 대해 상상해보는 것이 좋습니다

다음은 간단한 곱셈의 테스트 예입니다

```java
public void testMultiplication() {
	Dollar five = new Dollar(5);
	five.times(2);
	assertEquals(10, five.amount);
}
```

우리가 작성한 테스트는 아직 컴파일조차 되지 않습니다
이제 위 코드를 컴파일이라도 될 수 있도록 고쳐보도록 하겠습니다

다른 파일에 Dollar class를 작성해보도록 하겠습니다

- Dollar 클래스가 없음
- 생성자가 없음
- times(int) 메서드가 없음
- amount 필드가 없음

```java
class Dollar {

	public int amount;

	public Dollar(int amount) {
	}

	void times(int multiplier) {
	}
}
```

컴파일 에러를 잡았습니다! 이제 돌아가지만 여전히 테스트 코드는 실패합니다

이제 테스트를 통과할 해법이 마음에 안들지는 모르겠지만 당장의 목표는 해법을 구하는 것이 아니라 테스트를 통과하는 것일 뿐입니다

이제 코드를 고쳐봅시다!

```java
class Dollar {

	public int amount = 10;

	public Dollar(int amount) {
	}

	void times(int multiplier) {
	}
}
```

제가 제시할 최소한의 코드 변경입니다

드디어 컴파일과 테스트 코드가 통과합니다!
하지만 계속 진행하기 전에 일반화를 해야합니다

주기는 다음과 같습니다 (절대 잊으면 안됩니다)

1. 작은 테스트를 하나 추가합니다
2. 모든 테스트를 실행해서 테스트가 실패하는 것을 확인합니다
3. 조금 수정합니다
4. 모든 테스트를 실행해서 테스트가 성공하는 것을 확인합니다
5. 중복을 제거하기 위해 리팩토링을 합니다

우리는 지금 1번부터 4번 항목까지를 수행했습니다 이제 중복을 제거할 차례입니다
하지만 중복이 어디있단 말이죠? 보통 여러분은 중복을 찾기 위해 코드를 비교할 것입니다
하지만 이번 경우엔 중복이 테스트에 있는 데이터와 코드에 있는 데이터 사이에 존재합니다
(테스트 코드와 테스트를 진행하는 코드)

```java
class Dollar {

	public int amount = 10; // **중복**

	public Dollar(int amount) {
	}

	void times(int multiplier) {
	}
}
```

```java
public void testMultiplication() {
	Dollar five = new Dollar(5);
	five.times(2);
	assertEquals(10, five.amount); // **10 중복**
}
```

```java
class Dollar {

	public int amount;

	public Dollar(int amount) {
	}

	void times(int multiplier) {
		amount = 5 * 2; // *
	}
}
```

```java
public void testMultiplication() {
	Dollar five = new Dollar(5);
	five.times(2);
	assertEquals(10, five.amount);
}
```

테스트는 여전히 통과하고 테스트 막대 역시 초록색입니다
우리는 여전히 행복합니다 😊
이 단계가 너무 작게 느껴지십니까? 하지만 TDD의 핵심은 이런 작은 단계를 밟아야 한다는 것이 아니라, 이런 작은 단계를 밟을 능력을 갖추어야한다는 것입니다

다시 코드 중복을 줄이는 것으로 돌아가봅시다

```java
class Dollar {

	public int amount;

	public Dollar(int amount) {
		this.amount = amount; // *
	}

	void times(int multiplier) {
		// amount = amount * multiplier;
		amount *= multiplier; // *
	}
}
```

```java
public void testMultiplication() {
	Dollar five = new Dollar(5);
	five.times(2);
	assertEquals(10, five.amount);
}
```

- 5는 생성자에서 넘어온 값이므로 변수에 저장하면 times에 사용할 수 있습니다
- 2는 times의 multiplier의 값이므로 상수를 이 인자로 대체할 수 있습니다
- 저희는 자바 문법을 알기 때문에 더욱 더 간소화 시킨 연산자로 바꿀 수 있습니다 (*=)

축하합니다! 우리는 처음에 확인하였던 목록들중에, 2번째 목록을 완료하였습니다

- %5 + 10CHF = $10(환율이 2:1일 경우)
- ~~$5 * 2 = $10~~
- amount를 private 으로 만들기
- Dollar 부작용?
- Money 반올림?
