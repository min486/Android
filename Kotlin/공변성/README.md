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

## 🔥 Kotlin 공변성 (out T)

### 공변성

kotlin의 제네릭은 기본적으로 불변(Invariance) 이다

예시 : `List<String>`은 `List<Any>`의 하위 타입이 아니다

이는 타입 안전성을 보장하지만, 유연성은 떨어진다

<br>

공변성(out T)은 이러한 제약을 완화해준다

> 만약 A가 B의 하위 타입이면, `Producer<A>`도 `Producer<B>`의 하위 타입이 될 수 있도록 허용하는 개념

즉 제네릭 타입이 데이터를 생산(반환)만 하는 경우, 하위 타입 관계를 유지할 수 있게 된다

<br>

### `out T`의 의미

out T는 제네릭 타입 T가 출력 전용(생산자) 으로만 사용됨을 의미한다

- T는 반환(return)할 수 있지만, 입력(parameter)으로 받을 수는 없다
- 따라서 T 타입 객체를 생산만 가능, 소비는 불가능

👉 생산자(Producer) 역할을 하는 클래스에 적합한 키워드

<br>

### `Result<out T>`에서의 활용

다음은 로딩 상태를 표현하는 일반적인 패턴이다

```kotlin
sealed class Result<out T> {
    object Loading : Result<Nothing>()
    data class Success<out T>(val data: T) : Result<T>()
    data class Error(val exception: Throwable) : Result<Nothing>()
}
```

out T가 필요한 이유

- `Result.Success<String>`은 `Result<String>` 타입이다

- `Result.Error`는 `Result<Nothing>` 이지만,

  `Result<String>`, `Result<Int>`, `Result<User>` 등 모든 `Result<T>` 타입에 할당 가능해야 자연스럽다

<br>

만약 Result가 불변이라면,

`Result<Nothing>`을 `Result<String>` 타입 변수에 할당할 수 없어 컴파일 오류가 발생한다

<br>

out T 덕분에 가능한 일

- `Nothing`은 kotlin에서 모든 타입의 하위 타입(Bottom Type) 이며,
- `Result<Nothing>`은 모든 `Result<T>`의 하위 타입으로 간주된다

👉 따라서 Loading, Error는 어떤 T에도 사용 가능한 상태가 된다

<br>

### 요약

- out T는 생산자(Producer) 역할의 클래스에서 사용
- `Result<out T>` 덕분에 `Result<Nothing>`인 로딩/에러 상태를 모든 타입에 사용 가능
- `Nothin`은 모든 타입의 하위 타입이므로 공변성과 함께 사용할 때 강력한 표현력을 가짐 





























