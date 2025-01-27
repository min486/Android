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

## 🔥 데이터 클래스 (Data Class)

### 데이터 클래스

> 데이터 클래스는 주로 데이터를 보관하기 위해 사용된다

<br>

### 데이터 클래스 선언

앞에 data class 라는 키워드를 통해 데이터 클래스를 선언할 수 있다

```kotlin
fun main(args: Array<String>) {
  
    // data class 객체 생성
    val memo = Memo("Go to grocery", "Egg, Milk, Pork", false)
}

data class Memo(val title : String, val content : String, var isDone : Boolean)
```

<br>

### 데이터 클래스 특징

- 주생성자를 필수로 구현해야함
- val 이나 var 키워드를 사용해서 프로퍼티를 하나 이상 정의해야함
- 데이터 클래스를 사용하면 데이터를 저장하고 보관하고 다른데 쓰는데 필요한 함수들을 자동으로 생성해준다

👉 Data class는 `toString()`, `hashCode()`, `equals()`, `copy()` 메소드를 자동으로 정의하여 주기 때문에 간단하게 클래스를 만들 수 있다

