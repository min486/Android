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

## 🔥 인터페이스 (Interface)

### 인터페이스

> 인터페이스를 구현하는 클래스들이 똑같은 기능을 수행하게 강제함
>

<br>

### 인터페이스 특징

- 코드를 규격화하고 통제할 수 있음 (코드가 보기 좋음)

```kotlin
fun main(args: Array<String>) {
    val car = Car()  // 객체 생성
    car.drive()  // car is moving
    car.stop()  // car is stopping
  
    val bike = Bike()
    bike.drive()  // bike is moving
    bike.stop()  // bike is stopping
}

interface Vehicle {
    fun drive()
    fun stop()
    fun destroy() { println("vehicle is destroyed") }
}

class Car : Vehicle {
    override fun drive() {
        println("car is moving")
    } 
  
    override fun stop() {
        println("car is stopping")
    }
}

class Bike : Vehicle {
    override fun drive() {
        println("bike is moving")
    } 
  
    override fun stop() {
        println("bike is stopping")
    }
}
```

<br>

### 클래스를 상속받는 것과 인터페이스를 구현하는 것의 차이점

- 인터페이스는 여러개 구현 가능

```kotlin
interface Vehicle {
    fun drive()
    fun stop()
    fun destroy() { println("vehicle is destroyed") }
}

interface Color {
    fun showMeColor()
}

class Car : Vehicle, Color {
    override fun showMeColor() {
        println("red color")
    } 
  
    override fun drive() {
        println("car is moving")
    } 
  
    override fun stop() {
        println("car is stopping")
    }
}
```
