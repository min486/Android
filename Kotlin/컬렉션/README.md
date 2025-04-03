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

> 데이터를 효율적으로 관리하고 조작할 수 있도록 코틀린에서 제공하는 자료구조 클래스

<br>

### 컬렉션의 형태

- Immutable (변경 불가능)
  - 읽기 전용

- Mutable (변경 가능)
  - 요소의 삽입, 삭제, 수정이 가능함

<br>

### Kotlin의 주요 컬렉션

코틀린에서 가장 많이 사용되는 컬렉션은 List, Set, Map 이다

- List
  - 순서가 있는 자료구조로, 동일한 값을 여러 번 포함할 수 있음

- Set
  - 순서가 없으며, 중복된 요소를 허용하지 않는 자료구조

- Map
  - 키(key)와 값(value) 쌍으로 이루어진 자료구조
  - 키는 중복될 수 없으며, 각 키는 하나의 값만 가짐

<br>

### List 예제

```kotlin
// Immutable List
val fruitList = listOf("Banana", "Apple")
print("first fruit ${fruitList[0]}")  // first fruit Banana

fruitList[0] = "Melon"  // Immutable 이여서 값 변경 불가

// Mutable List
val mutableFruitList = mutableListOf("Banana", "Apple")
print("first fruit ${fruitList[0]}")  // first fruit Banana

mutableFruitList[0] = "Melon"
print("new first fruit ${fruitList[0]}")  // new first fruit Melon
```

<br>

### Set 예제

```kotlin
// Immutable Set
val numSet = setOf(1,1,2,3,3,4)
print(numSet)  // [1, 2, 3, 4]

// Mutable Set
val mutableNumSet = mutableSetOf(1,1,2,3,3,4)
print(mutableNumSet)  // [1, 2, 3, 4]

mutableNumSet.add(100)
mutableNumSet.remove(1)
print(mutableNumSet)  // [2, 3, 4, 100]
```

<br>

### Map 예제

```kotlin
// Immutable Map
val immutableMap = mapOf("name" to "min", "age" to 100, "height" to 200)
print(immutableMap["name"])  // min
print(immutableMap["no"])  // null

// Mutable Map
val mutableMap = mutableMapOf("name" to "min", "age" to 100, "height" to 200)
print(mutableMap["age"])  // 100

mutableMap["age"] = 5
print(mutableMap)  // {name=min, age=5, height=200}

mutableMap.put("hobby", "coding")
mutableMap.remove("name")
print(mutableMap)  // {age=5, height=200, hobby=coding}
```

