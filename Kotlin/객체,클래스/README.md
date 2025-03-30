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

## 🔥 객체, 클래스

### 객체

서로 연관된 것들을 묶어 만든 속성과 동작이 있는 집합체

<br>

### 필드와 메서드

- 객체 지향 프로그래밍에서 속성은 필드, 동작은 메서드 라고 한다
- 필드와 메서드로 구성된 것이 객체다

<br>

자동차 객체가 있다고 하면,

객체의 필드(속성)에는 색상, 모델명, 연료 등이 있다

객체의 메서드(동작)에는 전진한다, 멈춘다, 창문을 연다 등이 있다

<br>

### 클래스

객체를 만들기 위한 설계도

- 인스턴스는 실체가 있는 객체
- 인스턴스화는 인스턴스를 만드는 것

<br>

### 클래스 기본

```kotlin
// 객체 생성
fun main(args: Array<String>) {
    val car = Car("Red")  // Red가 Car 객체의 속성 값으로 지정됨
    val car = Car("Yellow")
  
    print("Color of the car : ${car.color}")  // Color of the car : Red
}

// Car 클래스 선언
class Car(val color : String)
```

