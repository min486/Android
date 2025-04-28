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

- 서로 연관된 속성과 동작을 하나로 묶은 집합체를 객체라고 한다

- 객체는 속성(필드) 과 동작(메서드) 으로 구성된다

<br>

예를 들어, 자동차 객체가 있다고 하면

- 필드(속성) : 색상, 모델명, 연료 등
- 메서드(동작) : 전진하기, 멈추기, 창문 열기 등이 있다

<br>

### 클래스

- 객체를 만들기 위한 설계도

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

<br>

### 생성자

- 객체를 생성할 때 호출되는 함수
- 객체의 초기 상태(필드 값)를 설정하는데 사용된다

<br>

생성자 종류

1. 주 생성자 (Primary Constructor)
   - 클래스 헤더에 선언하는 기본 생성자이다
   - constructor 키워드 없이 간결하게 정의할 수 있다

```kotlin
class Car(val color: String)
```

👉 Car 클래스는 color 라는 프로퍼티를 초기화하는 주 생성자를 가진다

<br>

2. 보조 생성자 (Secondary Constructor)
   - 클래스 본문 안에서 constructor 키워드를 사용해 추가로 정의할 수 있다
   - 주 생성자 외에 추가적인 방법으로 객체를 생성하고 싶을 때 사용한다

```kotlin
class Car {
    var color: String

    constructor(color: String) {
        this.color = color
    }
}
```

👉 this 키워드를 사용해 프로퍼티를 초기화한다

<br>

### 클래스 구성 요소

1. 메서드 (Method)
   - 클래스 안에서 동작을 정의하는 함수다

```kotlin
class Car(val color: String) {
    fun drive() {
        println("The $color car is driving")
    }
}
```

2. 프로퍼티 (Property)
   - 클래스가 가지는 데이터(속성) 이다
   - 기본적으로 val 또는 var로 선언한다

```kotlin
class Car(val color: String, var speed: Int)
```

3. 초기화 블록 (Init Block)
   - 객체가 생성될 때 자동으로 실행되는 코드 블록
   - 주 생성자에서 받은 인자값을 활용해 추가 초기화를 진행할 수 있다

```kotlin
class Car(val color: String) {
    init {
        println("A $color car has been created.")
    }
}
```

4. 상속 (Inheritance)
   - 부모 클래스의 속성과 기능을 자식 클래스에서 물려받을 수 있다
   - 클래스를 상속하려면 open 키워드를 붙여야 한다

```kotlin
open class Vehicle(val brand: String)

class Car(brand: String, val color: String) : Vehicle(brand)
```

👉 Car 클래스는 Vehicle 클래스를 상속받아 brand 속성을 함께 사용할 수 있다
