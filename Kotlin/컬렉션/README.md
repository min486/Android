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

## 🔥 컬렉션 (Collection)

### 컬렉션

> 자료구조를 쉽게 사용할 수 있도록 코틀린에서 제공하는 클래스

<br>

### 컬렉션 형태

- Mutable
  - 읽기 및 삽입, 삭제, 수정 가능
  - 변경 가능
- Immutable
  - 읽기 전용
  - 변경 불가능

<br>

### List, Set, Map

3가지는 컬렉션중에 가장 많이 사용된다

<br>

### List

순서가 있는 자료구조

```kotlin
val fruitList = listOf("Banana", "Apple")
print("first fruit ${fruitList[0]}")  // first fruit Banana

fruitList[0] = "Melon"  // Immutable 이여서 값 변경 불가

val mutableFruitList = mutableListOf("Banana", "Apple")
print("first fruit ${fruitList[0]}")  // first fruit Banana

mutableFruitList[0] = "Melon"
print("new first fruit ${fruitList[0]}")  // new first fruit Melon
```

<br>

### Set

<br>

### Map

