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

- 주로 데이터를 저장하거나 전달할 때 사용하는 클래스
- 데이터를 다루는데 필요한 기본 메서드들이 자동 생성된다

<br>

### 데이터 클래스 선언 방법

- data class 키워드를 사용해 선언한다
- 생성자 안에 val 또는 var로 프로퍼티를 하나 이상 선언해야 한다

```kotlin
fun main(args: Array<String>) {
  	// data class 객체 생성
    val memo = Memo("Go to grocery", "Egg, Milk", false)
  
  	println(memo)  // Memo(title=Go to grocery, content=Egg, Milk, isDone=false)
}

data class Memo(
  	val title : String,
  	val content : String,
  	var isDone : Boolean
)
```

<br>

### 데이터 클래스 특징

- 주 생성자 필수 : 반드시 주 생성자를 가져야 한다
- val/var 필수 : 주 생성자에 최소 하나 이상의 val 또는 var 프로퍼티를 정의해야 한다
- 자동 생성되는 메서드
  - `toString()` : 객체 정보를 문자열로 출력
  - `equals()` : 객체 내용 비교
  - `hashCode()` : 객체 해시 코드 생성
  - `copy()` : 객체를 복사하여 일부 프로퍼티만 변경 가능

