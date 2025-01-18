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

> 추상적인(실제적이지 않은) 클래스
>
> 그 자체로는 객체를 만들 수 없음 (인스턴스화 할 수 없음)

<br>

### 추상 클래스 특징

- 그 자체로 객체를 만들기 위함이 아니라 상속을 해주기 위한 부모 클래스
- abstract 키워드를 사용해서 사용
- abstract fun이 포함되어야함, abstract fun은 구현부가 없이 선언만 해줌
- 추상 클래스를 상속받는쪽에서 해당 abstract fun을 override하여 구현해야함
- 추상 클래스 내의 일반 함수는 구현 안해도됨

```kotlin
abstract class Vehicle {
  	abstract fun drive():
  
  	fun stop() {
      	println("stop")
    }
}
```

<br>

### 추상 클래스 역할

추상 클래스를 사용하면 상속받는 자식 클래스에서 반듯이 구현하게 강제화함

👉 프로젝트가 커짐에 따라 상속이 여러번 적용될 수 있음. 이때 어느 자식 클래스에서 꼭 무언가 구현해야할때 추상 클래스를 사용

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

### Kotlin의 Abstract Class 특징

abstract class는 여러 클래스의 공통적인 부분은 모아놓은 클래스이다

따라서, 그 클래스 내부의 일부 변수나 함수도 미완성 됐을 수 있다

(미완성된 변수나 함수에는 `abstract`가 앞쪽에 붙는다)

- abstract class는 그 자체로 미완성 됐으므로 인스턴스화가 불가능

- abstract class는 내부 변수나 함수 또한 abstract로 만들 수 있다

  👉 abstract로 만들어진 값들은 미완성 된 값들이므로, 인터페이스처럼 abstract class를 구현하는 곳에서 구현해야 한다

<br>

### Abstract Class의 한계점

abstract class는 절대로 바뀌지 않는 개념에 대한 공통 부분을 만들때 유용하다

하지만, 클라이언트 개발 시에는 모든 것이 바뀔 수 있어 사용할 일이 거의 없다

👉 변화에 대한 유연성을 가져가려면 abstract fun 보다 interface를 쓰는 것이 합리적이다

<br>

### Abstract Class와 Interface 차이점

- 추상 클래스

  - 상속과 필수구현을 위해
  - 대략적인 설계의 명세와 공통의 기능을 구현한 클래스 (구체적이지 않은 것)
  - 추상 클래스를 상속하는 하위 클래스는 추상 클래스의 내용을 더 구체화 해야 한다

- 인터페이스

  - 필수구현을 위해

  - 대략적인 설계의 명세를 구현하고 인터페이스를 상속하는 하위 클래스에서 이를 구체화

    (추상 클래스와 동일)

  - 프로퍼티의 상태 정보를 저장할 수 없다

    ```kotlin
    interface Vehicle {
      val name : String
      val color : String
      val weight : Double
    }
    
    interface Pet {
      val name : String = "puppy"  // 초기화 불가능
    }
    ```

