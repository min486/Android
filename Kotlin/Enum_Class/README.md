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

## 🔥 Enum Class

### Enum Class

- 열거형 상수를 정의할 때 사용
- 상수 하나하나가 객체처럼 동작하며, 각각 메서드나 프로퍼티를 가질 수 있다

<br>

### 특징

- 각 상수는 단일 인스턴스(singleton) 이다
- 이름은 보통 대문자를 사용한다
- 생성자나 프로퍼티도 가질 수 있으며, when 등 조건문에 잘 어울린다

<br>

### 예제

```kotlin
enum class Color {
    RED,
    GREEN,
    BLUE
}
```
