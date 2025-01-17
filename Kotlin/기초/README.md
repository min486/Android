<div align="center">
  <p>
    <img src="../README.assets/kotlin-hero.png">
  </p>
  <br>
  <h2>Kotlin</h2>
  <p>코틀린 관련 내용 정리</p>
  <br>
  <br>
</div>

## 🔥 기초

### val과 var의 차이

둘다 변수를 선언하는데 사용되는 키워드

- val
  - val은 value(값)의 약자로, 한 번 초기화되면 값을 변경할 수 없다
  - 불변(Immutable) 변수를 선언할 때 사용

- var
  - var은 variable(변수)의 약자로, 초기화 후에도 값을 변경할 수 있다
  - 가변(Mutable) 변수를 선언할 때 사용


<br>

변수 선언하는 방법

👉 var/val 변수명(상자) = 값(넣고 싶은 것)

<br>

변수 vs 상수

- 변하지는 않는 값이라면 ➡️ val 사용
- 앞으로 어떻게 사용될지 모르겠다 ➡️ val 사용
  - 보통 변수는 val로 선언하고 나중에 바꿀 일이 있으면 그때 var로 바꾼다
  - 변수를 사용하는 시점이 선언한 부분과 멀어지면 확인이 어렵기 때문
