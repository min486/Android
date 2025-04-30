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

## 🔥 객체지향 프로그래밍 (OOP)

### 객체 지향 프로그래밍

- OOP(Object Oriented Programming)는 프로그램을 다수의 객체로 만들고,

  객체 간의 상호작용을 통해 기능을 수행하는 방식

- 목적 : 재사용성, 유지보수성, 확장성을 높이는 것

- 코틀린은 객체지향 언어

<br>

### 객체지향 프로그래밍의 4가지 특징

1. 추상화 (Abstraction)

- 공통성을 모아서 추출하는 것

- 공통된 동작을 인터페이스 또는 추상 클래스로 정의한다

- 예시 : Vehicle 추상 클래스를 만들어 Car, Bike가 이를 상속받게함

```kotlin
abstract class Vehicle {
    abstract fun move()
    fun stop() = println("Stopping...")
}
class Car : Vehicle() {
    override fun move() = println("Car is moving")
}
```

<br>

2. 상속 (Inheritance)

- 부모 클래스의 속성과 기능을 자식 클래스에 물려줌
- 중복 코드를 줄이고, 일관성 있는 구조를 만든다

```kotlin
open class Vehicle(val name: String) {
    fun stop() = println("$name stops")
}
class Bike(name: String) : Vehicle(name)
```

<br>

3. 다형성 (Polymorphism)

- 같은 이름의 메서드가 상황에 따라 다르게 동작함
- 오버라이딩(overriding)을 통해 자식 클래스마다 다른 구현 가능

```kotlin
open class Animal {
    open fun sound() = println("Some sound")
}
class Dog : Animal() {
    override fun sound() = println("Bark")
}
```

- 다형성은 아래처럼 활용됨

```kotlin
fun makeSound(animal: Animal) {
    animal.sound()
}
makeSound(Dog())  // Bark
```

<br>

4. 캡슐화 (Encapsulation)

- 관련 데이터를 하나로 묶고, 외부로부터 직접 접근을 제한하는 것
- 코틀린에서는 private 등의 접근 제한자를 사용

```kotlin
// 외부에서 balance에 직접 접근할 수 없음 → 객체 내부 로직 보호 가능
class Account(private var balance: Int) {
    fun deposit(amount: Int) { balance += amount }
    fun getBalance(): Int = balance
}
```
