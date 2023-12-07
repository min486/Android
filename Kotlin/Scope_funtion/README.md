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

## 🔥 Scope function (범위 지정 함수)

### 정의

> 코틀린 표준 라이브러리에서 제공하는 확장함수
>
> 객체의 context 내에서, 실행 가능한 코드 블럭을 만드는 함수

👉 호출하면 임시 범위가 생성되며, 이 범위 안에서는 이름 없이 객체에 접근가능

<br>

<img src="../README.assets/scope.png" alt="scope" align="left" style="zoom:67%;" />

### 차이점

- apply, also : 수신객체 (확장 함수가 호출되는 대상이 되는 값(객체)) 를 return

  - apply : 객체 초기화
  - also : 수신객체를 명시적으로 사용하고 싶을 때, 로그를 남길 때

- run, let : 수신객체를 명시하지 않고, 람다의 본문 안에서 해당 객체의 메서드 호출 가능하게 함

  반환하는 값이 객체가 아닌 스코프 내에 실행된 값 (람다의 결과)

  - run : 객체 초기화 하고, 리턴 값이 있을 때
  - let : null 체크를 해야할 때, 지역 변수를 명시적으로 표현할 때

- with : 객체 초기화, 람다 리턴 값이 필요 없을 때
