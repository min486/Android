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

## 🔥 제네릭 (Generic)

### 제네릭

- 제네릭은 클래스나 함수에서 사용할 타입을 외부에서 결정할 수 있게 하는 기능
- 다양한 타입에 대해 중복 코드 없이 재사용 가능한 구조를 제공

```kotlin
class Box<T>(private val item: T) {
    fun getItem(): T = item
}

val stringBox = Box("Hello")
val intBox = Box(123)
```

<br>

### 사용 이유

- 타입별로 클래스를 여러개 만들 필요 없이, 하나의 클래스(또는 함수)로 여러 타입을 처리할 수 있음
- 코드 재사용성과 타입 안정성 증가

```kotlin
class Printer<T>(private val value: T) {
    fun print() {
        println(value)
    }
}
```

<br>

### 예시

- 제네릭 클래스
  - 클래스 이름 뒤, 꺽쇠 괄호(< >) 안에 type parameter 이름을 정의한다

```kotlin
class Container<T>(val data: T)
val stringContainer = Container("text")
val intContainer = Container(42)
```

- 제네릭 함수

  - fun 키워드 뒤, 꺽쇠 괄호(< >) 안에 type parameter 이름을 선언하고,

    이를 수신 객체 타입, 함수 파라미터 타입, 반환 타입 등에 사용할 수 있다

```kotlin
fun <T> printList(items: List<T>) {
    for (item in items) println(item)
}

fun <T> List<T>.typeTest(item: T): List<T> {
    println(item)
    return this
}
```

<br>

### 제네릭 타입

타입 파라미터로 사용하는 변수들은 보통 아래와 같이 사용한다

| 타입 변수 | 의미       |
| --------- | ---------- |
| T         | Type       |
| E         | Element    |
| K, V      | Key, Value |
| N         | Number     |

<br>

### 제네릭과 상속

```kotlin
open class Box<T>(val item: T)

class FruitBox : Box<String>("Banana")  // 구체적인 타입으로 상속
class CustomBox<T> : Box<T>(item = ...)  // 타입 파라미터 전달
```

<br>

### reified 키워드

- 코틀린에서는 제네릭 타입 정보가 런타임에 지워짐
- 하지만 inline 함수 + reified 키워드를 사용하면 타입 정보를 런타임에도 활용 가능

```kotlin
inline fun <reified T> isType(value: Any): Boolean {
    return value is T
}

println(isType<String>("hi"))  // true
println(isType<Int>("hi"))     // false
```

