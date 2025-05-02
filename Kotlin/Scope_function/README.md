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

## 🔥 범위 지정 함수 (Scope Function)

### 범위 지정 함수

- 코틀린의 표준 라이브러리에서 제공하는 확장 함수
- 객체를 임시 스코프로 감싸서, this 또는 it을 통해 객체에 접근하고 작업을 수행함
- 반복적인 객체 접근이나 초기화, null 처리 등을 더 간결하고 명확하게 표현할 수 있음

👉 객체에 간결하게 접근하고 작업할 수 있도록, 임시 스코프를 생성하는 코틀린의 확장 함수

<br>

### 종류 및 차이점

| 함수  | 객체 접근 방식 | 리턴 값   | 사용 목적                  | 대표 상황                            |
| ----- | -------------- | --------- | -------------------------- | ------------------------------------ |
| apply | this           | 객체 자신 | 객체 초기화                | builder 패턴, View 설정              |
| also  | it             | 객체 자신 | 부가 작업                  | 로깅, 디버깅                         |
| run   | this           | 람다 결과 | 객체 사용 후 결과 리턴     | val result = user.run { name + age } |
| let   | it             | 람다 결과 | null 체크, 임시 범위       | nullable?.let { ... }                |
| with  | this           | 람다 결과 | 특정 객체로 여러 작업 수행 | with(viewModel) { ... }              |

👉 this는 생략 가능, 수신 객체의 멤버에 직접 접근 가능

👉 it은 생략 불가능, 수신 객체를 변수처럼 다룰 수 있음

<br>

<img src="../README.assets/scope.png" alt="scope" align="center" width="80%" />

<br>

### 예시 코드

- apply

```kotlin
// 객체 초기화 후 반환
val textView = TextView(context).apply {
    text = "Hello"
    textSize = 16f
}
```

- also

```kotlin
// 로깅이나 부가 작업
val list = mutableListOf("a", "b").also {
    println("Initial list: $it")  // it은 list
}
```

- run

```kotlin
// 객체 사용 후 결과 반환
data class User(var firstName: String, var lastName: String)

val fullName = User("John", "Doe").run {
    "$firstName $lastName"
}

println(fullName)  // John Doe
```

👉 `User("John", "Doe")`가 수신 객체

👉 run 안에서는 `this.firstName`, `this.lastName`을 생략하고 사용 가능

👉 마지막 줄인 `"$firstName $lastName"`이 반환 값이 되어 `fullName`에 저장됨

- let

```kotlin
// 로깅이나 부가 작업
val list = mutableListOf("a", "b").also {
    println("Initial list: $it")  // it은 list
}
```

- with

```kotlin
val user = User("John", "Doe")
val fullName = with(user) {
    "$firstName $lastName"
}

println(fullName)  // John Doe
```

👉 일반 함수로 수신 객체를 인자로 전달
