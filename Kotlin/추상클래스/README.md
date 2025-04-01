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

## 🔥 추상 클래스 (Abstract Class)

### 추상 클래스 

> 일부 함수는 기본 구현을 제공하고, 일부 함수는 상속받는 자식 클래스에서 반드시 구현해야 한다

👉 상속 계층을 만들고 싶으면 추상 클래스 사용

👉 프로젝트가 커짐에 따라 상속이 여러번 적용될 수 있다. 이때 어느 자식 클래스에서 꼭 무언가 구현해야할때 추상 클래스 사용

<br>

### 추상 클래스 특징

- 구현이 없는 함수를 정의할 수 있으며, 이를 상속한 클래스에서 구현해야 한다

  👉 인터페이스도 동일

- 직접 인스턴스를 만들 수 없고, 상속한 클래스에서 객체를 생성해야 한다

  👉 인터페이스도 동일
- abstract 키워드를 사용해서 정의한다
- 단일 상속만 가능
- 생성자 사용 불가능
- abstract fun이 포함되어야 하고, abstract fun은 구현부가 없이 선언만 해준다
- 추상 클래스를 상속받는쪽에서 해당 abstract fun을 override하여 구현해야함
- 추상 클래스 내의 일반 함수는 구현을 안해도 된다

```kotlin
abstract class Vehicle {
    abstract fun drive():
  
    fun stop() {
        println("stop")
    }
}
```

<br>

### 사용 예제

```kotlin
fun main(args: Array<String>) {
  
    val venue = Venue()
  
    venue.startDrive()  // start drive
    venue.printCarName()  // this is venue
}

abstract class Vehicle {
    fun startDrive() {
        println("start drive")
    }
  
    abstract fun printCarName():
}

class Venue : Vehicle() {
    override fun printCarName() {
        println("this is venue")
    }
}
```

<br>

### 추상 클래스와 인터페이스 차이점

- 추상 클래스 (Abstract Class)

  - 단일 상속만 가능
  - 생성자 사용 가능
  - 상속 계층을 만들고 싶으면 추상 클래스 사용

    👉 ex) 개(Dog)는 동물(Animal) 이다

- 인터페이스 (Interface)

  - 다중 상속 가능

  - 생성자 사용 불가능

  - 다양한 클래스에 기능을 추가하고 싶으면 인터페이스 사용

    👉 ex) 자동차(Car)는 운전할 수 있다(Drivable)
  

