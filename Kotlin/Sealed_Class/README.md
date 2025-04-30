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

## 🔥 Sealed Class

### Sealed Class

- 상속 가능한 자식 클래스의 범위를 제한하기 위한 추상 클래스
- when 문에서 강력한 타입 체크 기능 제공

<br>

### 특징

- 같은 파일 내에서만 하위 클래스 정의 가능
- 직접 인스턴스화 불가능
- 하위 클래스가 무엇이 있는지 컴파일러가 알기 때문에, when 문에서 else 생략 가능

<br>

### 예제

```kotlin
sealed class Shape

class Circle(val radius: Double): Shape()
class Rectangle(val width: Double, val height: Double): Shape()

fun describe(shape: Shape): String = when(shape) {
    is Circle -> "Circle with radius ${shape.radius}"
    is Rectangle -> "Rectangle of size ${shape.width} x ${shape.height}"
}
```
