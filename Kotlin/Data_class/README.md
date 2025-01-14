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

## 🔥 Data class

### Data class

> 데이터 클래스는 주로 데이터를 저장하는데 사용된다

<br>

### Data class 특징

Data class는 `toString()`, `hashCode()`, `equals()`, `copy()` 메소드를 자동으로 정의하여 주기 때문에 간단하게 클래스를 만들 수 있다

```kotlin
data class User(
    val name: String,
    val age: Int
)
```

👉 class 앞에 data라는 접두어를 붙이면 사용 가능

