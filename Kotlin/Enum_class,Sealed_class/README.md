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

## 🔥 Enum class / Sealed class

### Enum class

> 열거형 타입을 정의할 수 있다. 명확한 상수 값을 갖는 타입을 나타낼 수 있다 
>
> 특정 값을 single instance로서 하나의 객체만 제한적으로 사용할 수 있으며, 생성자의 형태도 동일해야만 한다

<br>

### 사용 예제

```kotlin
// enum class 안에는 상수를 나타내는 대문자로 나타낸다
enum class Color {
   RED,
   BLUE,
   GREEN
}
```

<br>

### Sealed class

> 추상 클래스(abstract class)로 상속받는 자식 클래스의 종류를 제한하는 특성을 가지고 있다
>

<br>

### Sealed class 특징

- Sealed class의 하위 클래스들은 동일한 파일(같은 패키지 내)에 정의되어야 한다. 서로 다른 파일에서 정의할 수 없다
- 직접 인스턴스 생성이 불가능하다

<br>

### Sealed class 장점

when을 사용할 때 Sealed class를 사용하면 else 구문을 추가하지 않아도 된다

👉 컴파일러에서 Sealed class의 자식 클래스가 어떤 것이 있는지 알기 때문

<br>

### 사용 예제

```kotlin
Sealed class Shape

class Rectangle : Shape()
class Circle : Shape()
class Triangle : Shape()

fun whatIsShape(shape : Shape) : String{
  return when(shape){
    is Rectangle -> "This is Rectangle"
    is Circle -> "This is Circle"
    is Triangle -> "This is Triangle"
  }
}
```
