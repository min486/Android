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

## 🔥 객체와 클래스

### 객체

- 서로 연관된 속성과 동작을 하나로 묶은 집합체를 객체라고 한다.

- 객체는 속성(필드) 과 동작(메서드) 으로 구성된다

<br>

예를 들어, 자동차 객체가 있다고 하면

- 필드(속성) : 색상, 모델명, 연료 등
- 메서드(동작) : 전진하기, 멈추기, 창문 열기 등이 있다

<br>

### 클래스

- 객체를 만들기 위한 설계도다.

- 클래스를 기반으로 만들어진 구체적인 객체를 인스턴스라고 한다

- 클래스로부터 객체를 생성하는 과정을 인스턴스화라고 한다.

<br>

### 클래스와 객체 차이

클래스는 객체를 만들기 위한 설계도, 객체는 클래스를 기반으로 만들어진 실체

<br>

### 클래스와 객체 생성 예시

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

