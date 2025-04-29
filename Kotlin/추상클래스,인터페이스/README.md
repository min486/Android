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

## 🔥 추상 클래스 vs 인터페이스

### 추상 클래스 (Abstract Class)

- 일부 함수는 구현하고, 일부는 자식 클래스에서 구현하게 만드는 클래스
- 주로 상속 계층이 필요한 경우에 사용

<br>

### 특징

- abstract 키워드로 선언

- 일반 메서드 구현 가능 (abstract 아닌 함수)

- 직접 인스턴스화 불가

- 단일 상속만 가능

- 생성자 사용 가능

- 추상 메서드(abstract fun)는 반드시 오버라이드(override) 필요

  👉 구현 불가능, 선언만 가능

```kotlin
abstract class Animal {
    abstract fun makeSound()  // 구현 불가능
    
    fun breathe() {  
        println("Breathing...")  // 구현 가능
    }
}

class Dog : Animal() {
    override fun makeSound() {
        println("Bark")
    }
}
```

<br>

### 인터페이스 (Interface)

- 클래스들이 공통으로 갖춰야 할 기능(동작)을 정의하는 구조

- 주로 기능을 추가하거나, 기능 목록을 정의할 때 사용

<br>

### 특징

- interface 키워드로 선언

- 다중 상속 가능

- 생성자 없음

- 구현부 있는 함수 정의 가능

- 구현부 없는 함수는 반드시 오버라이드 필요

  👉 인터페이스를 구현한 클래스는, 인터페이스에 정의된 함수들을 반드시 구현해야함

```kotlin
interface Flyable {
    fun fly()
    fun land() { println("Landing...") }
}

class Bird : Flyable {
    override fun fly() { println("Flapping wings") }
}
```

<br>

### 추상 클래스와 인터페이스 차이

| 항목    | 추상 클래스                       | 인터페이스                               |
| ------- | --------------------------------- | ---------------------------------------- |
| 상속    | 단일 상속만 가능                  | 다중 상속 가능                           |
| 생성자  | 사용 가능                         | 사용 불가                                |
| 목적    | 상속 구조 정의                    | 공통 기능 정의                           |
| 구현    | 일반 메서드 구현 가능             | 구현부 있는 함수 정의 가능               |
| 사용 예 | 동물 계층 구조 (예: Animal ➡️ Dog) | 특정 기능 정의 (예: Drivable, Clickable) |
