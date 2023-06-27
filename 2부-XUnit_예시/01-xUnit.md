<aside>
💡 **들어가기 앞서**

1. xUnit 아키텍처는 파이썬에서 아주 매끄럽게 도출되기 때문에 파이썬으로 작성
2. 테스트 프레임워크의 로직은 1부의 money예제보다 더 복잡하다.
3. 자기 참조 프로그래밍에 대한 전산학 실습으로 생각할 수 있다.
4. 테스트 케이스를 작성하기 위해 사용할 프레임워크를 테스트하기 위한 테스트 케이스를 작성해야합니다. ⇒ 프레임워크가 없으므로 수동 검증
</aside>

- 테스트 목록
    - **테스트 메서드 호출**
    - setUp 호출
    - tearDown 호출
    - 테스트 메서드가 실패하더라곧 tearDown 호출
    - 여러 개 테스트 실행
    - 수집된 결과 출력

- 메서드가 호출되면 true, 아니면 false를 반환할 작은 프로그램이 필요하다.
- wasRun ⇒ 메서드가 실행되었는지 라는 네이밍

```python
class wasRun:
    def __init__(self,name):
        self.wasRun= None
    def testMethod(self):
        self.wasRun = 1

# main.py
test = wasRun("testMethod")
print(test.wasRun)
test.testMethod()
print(test.wasRun)
```

- 진짜 인터페이스인 run() 메서드 사용

```python
# wasRun
def run(self):
      self.testMethod()

if __name__ == '__main__':
    test = WasRun("testMethod")
    print(test.wasRun)
    test.run()
    print(test.wasRun)
```

- testMethod()를 동적으로 호출

```python
# wasRun
def run(self):
    method = getattr(self, self.name)
    method()
```

⇒ 테스트 케이스의 이름과 같은 문자열을 갖는 필드가 주어지면, 함수로 호출될 때 해당 메서드를 호출하게끔 하는 객체를 얻어낼 수 있다. (그래서 동적 호출)

- wasRun의 역할
    - 메서드가 호출되었는지 아닌지
    - 메서드를 동적으로 호출

- TestCase라는 상위 클래스를 만듭니다.

```python
class TestCase:
    def __init__(self, name):
        self.name = name

    def run(self):
        method = getattr(self, self.name)
        method()
```

```python
class WasRun(TestCase):
    def __init__(self, name):
        self.wasRun = None
        TestCase.__init__(self, name)

    def testMethod(self):
        self.wasRun = 1
```

⇒ run 메서드는 상위 클래스에 있는 속성만 사용하기 때문에 상위 클래스로 올립니다.

- 지금까지 main에

```python
# main
test = WasRun("testMethod")
print(test.wasRun)
test.run()
print(test.wasRun)
```

⇒ 이렇게 print로 찍기도 귀찮다.

```python
# testCaseTest
class TestCaseTest(TestCase):
    def testRunning(self):
        test = WasRun("testMethod")
        assert (not test.wasRun)
        test.run()
        assert (test.wasRun)
```

```python

# main
TestCaseTest("testRunning").run()
```

⇒ 이것을 통해서 확인한다.