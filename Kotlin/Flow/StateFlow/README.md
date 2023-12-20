<div align="center">
  <p>
    <img src="../../README.assets/kotlin-hero.png">
  </p>
  <br>
  <h2>Kotlin</h2>
  <p>코틀린 관련 내용 정리</p>
  <br>
  <br>
</div>


## 🔥 StateFlow

### Flow와 StateFlow

여러 데이터 흐름(Flow)을 하나로 합쳐 하나의 데이터 흐름(Flow)으로 만든다

아래 예시에서는 Flow가 3개 있고 이걸 합쳐 하나의 Flow를 만든다

<img src="../../README.assets/stateflow.png" alt="flow" align="center" width="30%" />

- 하나로 만들어진 Flow는 UI에서 사용되기 위해 StateFlow로 변환되어야 한다

- UI에서 이 StateFlow를 구독하여 항상 최신 데이터를 발행받는다

👉 이것이 가능하기 위해서 Flow를 StateFlow로 변환하는 로직이 필요하다

- StateFlow가 항상 Flow를 구독하고 있으면 메모리 누수가 생기므로

👉 이 StateFlow가 살아있어야 하는 CoroutineScope를 명시해야 한다

✅ 이를 `stateIn` 함수를 통해 구현할 수 있다

<br>

### stateIn을 사용하여 Flow > StateFlow 변환

```kotlin
// 초기 저장값은 emptyList() 이고, 수집이 중단되고 5초간만 발행되고, 
// ViewModel의 생명주기만큼만 구독받는 행동을 하는 StateFlow가 만들어짐

val contentList = contentRepository.loadList()
  .stateIn(
    initialValue = emptyList(),
    started = SharingStarted.WhileSubscribed(5000),
    scope = viewModelScope
  )
```

- initialValue : StateFlow에 저장될 초기값을 설정한다
- started : Flow로부터 언제부터 구독을 할지 명시할 수 있다
  - WhileSubscribed는 collector가 없어졌을 때 지정된 시간 이후 StateFlow 발행을 멈추도록 만드는 값
  - 따라서, collector가 없어진 후 5초 후에 동작을 멈추도록 만들기 위해 `SharingStarted.WhileSubscribed(5000)` 설정

- scope : StateFlow가 Flow로부터 데이터를 구독받을 CoroutineScope을 명시한다
  - StateFlow가 Flow로부터 구독을 하는 범위는 ViewModel이 살아있을 때까지만이다
  - 따라서, viewModelScope을 scope(구독 범위)로 넘긴다

