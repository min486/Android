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

## 🔥 lateinit & lazy (지연 초기화)

### 원시 자료형, 비원시 자료형

- Primitive (원시 자료형)

  : 메모리 공간에 바로 값이 저장되는 타입

  - Int
  - Double
  - Float
  - Boolean

- Non Primitive (비원시 자료형)

  : 객체를 참조하는 타입

  - String
  - Array
  - Class (사용자 정의 타입)

<br>

### lateinit

- 나중에 초기화할 변수를 선언할 때 사용
- 변수를 선언할 때 바로 값을 할당하지 않고, 필요한 시점에 값 할당이 가능

<br>

사용법

- lateinit은 var 변수에만 사용 가능하다
- Non Primitive(비원시) 타입에만 사용 가능하다
- Nullable 타입과는 함께 사용할 수 없다 (예: String?)

```kotlin
lateinit var lunch: String
lunch = "waffle"

println(lunch)  // waffle
```

<br>

주의사항

- lateinit을 사용한 변수를 초기화 전에 사용하면 예외가 발생한다
- 반드시 초기화하고 사용해야 한다

<br>

### lazy

- 변수를 처음 사용할 때 초기화하는 방법
- 주로 무거운 객체 초기화에 사용

<br>

사용법

- lazy는 val 상수에만 사용 가능하다
- `lazy {}` 블록 안에 초기화 코드를 작성한다
- 처음 호출될 때만 초기화 블록이 실행되고, 그 이후로는 실행되지 않는다

```kotlin
val lazyBear: String by lazy {
  	println("Bear is coming")
  	"bear"  // string 할당
}

println(lazyBear)  // Bear is coming
                   // bear

println(lazyBear)  // bear
```

👉 `println(lazyBear)` 를 처음 호출할 때, "Bear is coming"을 출력하고 "bear"를 초기화한다

👉 두번째 호출부터는 초기화 로직을 실행하지 않고, "bear" 값만 반환한다

